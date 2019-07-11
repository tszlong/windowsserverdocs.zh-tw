---
title: 建立您的災害復原方案
description: 了解如何建立您的 RDS 部署災害復原方案。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: e7bfe19258662a8e334ea0476689d8e860bfc8e5
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743903"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>建立您的 RDS 災害復原方案

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

您可以在 Azure Site Recovery 中建立災害復原方案，將容錯移轉程序自動化。 將所有 RDS 元件 VM 新增至復原方案。

請在 Azure 中使用下列步驟以建立您的復原方案：

1. 在 Azure 入口網站中開啟 Azure Site Recovery 保存庫，然後按一下 [復原方案]  。
2. 按一下 [建立]  並輸入方案的名稱。
3. 選取您的 [來源]  與 [目標]  。 目標可以是次要 RDS 站台或 Azure。
4. 選取裝載您 RDS 元件的 VM，然後按一下 [確定]  。

下列各節提供為不同類型 RDS 部署建立復原方案的其他資訊。

## <a name="sessions-based-rds-deployment"></a>工作階段型的 RDS 部署

針對 RDS 工作階段型部署，請將 VM 分組以便其依序顯示：

1. 容錯移轉群組 1：工作階段主機 VM
2. 容錯移轉群組 2：連線代理人 VM
3. 容錯移轉群組 3：Web 存取 VM

您的方案看起來會像這樣： 

![工作階段型 RDS 部署的災害復原方案](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>集區桌面 RDS 部署

針對具有集區桌面的 RDS 部署，請將 VM 分組以便其依序顯示，並新增手動步驟和指令碼。

1. 容錯移轉群組 1：RDS 連線代理人 VM
2. 群組 1 手動動作：更新 DNS

   在連線代理人 VM上以更高階的權限模式執行 PowerShell。 執行下列命令並等候數分鐘，以確保 DNS 更新為新值：

   ```
   ipconfig /registerdns
   ```
3. 群組 1 指令碼：新增虛擬主機

   修改以下指令碼以針對雲端中的每部虛擬主機執行。 通常，在您將虛擬主機新增至連線代理人之後，您需要重新啟動主機。 請先確認主機沒有擱置中的重新開機再執行指令碼，否則指令碼將會失敗。

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. 容錯移轉群組 2：範本 VM
5. 群組 2 指令碼 1：關閉範本 VM
   
   復原至次要站台時，範本 VM 將會啟動，但由於該 VM 是 Sysprep VM，因此無法完全啟動。 此外，RDS 需要關閉 VM，以從中建立集區 VM 設定。 因此，我們需要將其關閉。 如果您只有一部 VMM 伺服器，則範本 VM 名稱在主要和次要站台上都會相同。 因此，我們使用由以下指令碼中 *Context* 變數指定的 VM 識別碼。 如果您有多個範本，請將這些範本全部關閉。

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. 群組 2 指令碼 2：移除現有的 VM 集區

   您需要從連線代理人中移除主要站台的集區 VM，以便於次要站台建立新的 VM。 在此情況下，您必須指定要建立集區 VM 的確切主機。 請注意，這只會從集合刪除 VM。

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops; 
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. 群組 2 手動動作：指派新的範本

   您需要將新範本指派給集合的連線代理人，以便您在復原站台上建立新的 VM 集區。 請前往 RDS 連線代理人並識別集合。 編輯內容，並將新的 VM 映像指定為其範本。
8. 群組 2 指令碼 3：重新建立所有集區 VM

   透過連線代理人在復原站台上重新建立集區 VM。 在此情況下，您必須指定要建立集區 VM 的確切主機。

   集區 VM 名稱必須為是唯一且使用前置詞和後置詞。 如果 VM 名稱已經存在，指令碼將會失敗。 此外，如果主要端 VM 的編號是 1 到 5，則復原站台的編號會從 6 繼續。

   ```powershell
   ipmo RemoteDesktop; 
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1} 
   ```
9. 容錯移轉群組 3：Web 存取和閘道伺服器 VM

復原方案看起來會像這樣：

![具有集區桌面之 RDS 部署的災害復原方案](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>個人桌面 RDS 部署

針對具有個人桌面的 RDS 部署，請將 VM 分組以便其依序顯示，並新增手動步驟和指令碼。

1. 容錯移轉群組 1：RDS 連線代理人 VM
2. 群組 1 手動動作：更新 DNS

   在連線代理人 VM上以更高階的權限模式執行 PowerShell。 執行下列命令並等候數分鐘，以確保 DNS 更新為新值：

   ```
   ipconfig /registerdns
   ```
3. 群組 1 指令碼：新增虛擬主機
      
   修改以下指令碼以針對雲端中的每部虛擬主機執行。 通常，在您將虛擬主機新增至連線代理人之後，您需要重新啟動主機。 請先確認主機沒有擱置中的重新開機再執行指令碼，否則指令碼將會失敗。

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. 容錯移轉群組 2：範本 VM
5. 群組 2 指令碼 1：關閉範本 VM
   
   復原至次要站台時，範本 VM 將會啟動，但由於該 VM 是 Sysprep VM，因此無法完全啟動。 此外，RDS 需要關閉 VM，以從中建立集區 VM 設定。 因此，我們需要將其關閉。 如果您只有一部 VMM 伺服器，則範本 VM 名稱在主要和次要站台上都會相同。 因此，我們使用由以下指令碼中 *Context* 變數指定的 VM 識別碼。 如果您有多個範本，請將這些範本全部關閉。

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. 容錯移轉群組 3：個人 VM
7. 群組 3 指令碼 1：移除現有並新增個人 VM

   從連線代理人中移除主要站台的個人 VM，以便於次要站台建立新的 VM。 您需要擷取 VM 的指派，並將虛擬機器重新新增至具有指派雜湊的連線代理人。 這只會從集合中移除並重新新增個人 VM。 個人桌面配置將會匯出，並重新匯入集合中。

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName CEODesktops; 
   Export-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 

   Foreach($vm in $desktops){
     Remove-RDVirtualDesktopFromCollection -CollectionName CEODesktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   
   Import-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 
   ```
8. 容錯移轉群組 3：Web 存取和閘道伺服器 VM

您的方案看起來會像這樣： 

![個人桌面 RDS 部署的災害復原方案](media/rds-asr-personal-desktops-drplan.png)
