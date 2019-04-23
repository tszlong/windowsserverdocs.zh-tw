---
title: 建立您的災害復原計劃
description: 了解如何建立您的 RDS 部署災害復原計畫。
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
ms.openlocfilehash: 8ad759a73e4a0ce1dc28f2b8e8d80f4365895430
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879499"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>建立 RDS 的災害復原計劃

>適用於：Windows Server （半年通道），Windows Server 2016

您可以在 Azure Site Recovery，將容錯移轉程序自動化的災害復原計劃。 將 RDS 元件的所有 Vm 都新增至復原方案。

若要建立您的復原方案，在 Azure 中使用下列步驟：

1. 在 Azure 入口網站中，開啟 Azure Site Recovery 保存庫，然後按一下**復原計劃**。
2. 按一下 **建立**輸入計劃的名稱。
3. 選取您**來源**並**目標**。 目標可能是次要 RDS 網站或 Azure。
4. 選取的 Vm，裝載您的 RDS 元件，然後按一下**確定**。

下列各節提供有關建立不同類型的 RDS 部署中的復原計劃的其他資訊。

## <a name="sessions-based-rds-deployment"></a>工作階段為基礎的 RDS 部署

針對 RDS 工作階段式部署中，將 Vm 分組讓在序列中出現：

1. 容錯移轉群組 1-工作階段主機 VM
2. 容錯移轉群組 2-連線代理人 VM
3. 容錯移轉群組 3-Web 存取 VM

您的方案看起來像這樣： 

![工作階段為基礎的 RDS 部署嚴重損壞復原計畫](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>集區的桌面 RDS 部署

針對集區桌面與 RDS 部署中，Vm 群組，以便在出現的順序，新增的手動步驟和指令碼。

1. 容錯移轉群組 1-RDS 連線代理人 VM
2. 群組 1 的手動動作-更新 DNS

   在提高權限的模式中執行 PowerShell，連線代理人 VM 上。 執行下列命令，並等候數分鐘，以確保 DNS 更新為新值：

   ```
   ipconfig /registerdns
   ```
3. 群組 1 的指令碼-新增虛擬化主機

   修改下列指令碼執行在雲端中的每個虛擬化主機。 通常您新增到連線代理人虛擬化主機之後，您需要重新啟動主機。 請確保主機不需要重新開機擱置中之前的指令碼執行，否則它將會失敗。

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. 容錯移轉群組 2-範本 VM
5. 群組 2 的指令碼 1-開啟範本 VM 關閉
   
   範本 VM 至次要站台復原時，將會啟動，但它已執行過 sysprep VM，並完全無法啟動。 也 RDS 要求 VM 必須從它建立集區的 VM 組態的關機。 因此，我們需要將它關閉。 如果您有單一 VMM 伺服器時，範本的 VM 名稱會是相同的主要和次要資料庫。 因此，使用 VM 識別碼所指定*內容*以下指令碼變數。 如果您有多個範本，請將它們全部關閉。

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. 群組 2 的指令碼 2-移除現有的 Vm 集區

   您需要從連線代理人移除主要站台上的 Vm 集區，因此可以在次要網站上建立新的 Vm。 在此情況下，您需要指定確切的主控件，要在其中建立集區的 VM。 請注意，這會從集合刪除 Vm。

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops; 
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. 群組 2 手動動作指派新的範本

   您需要將新的範本指派給集合連線代理人，因此您可以在復原站台上建立新的 Vm 集區。 請移至 RDS 連線代理人，並識別集合。 編輯內容，並為其範本中指定新的 VM 映像。
8. 群組 2 的指令碼 3-重新建立所有集區的 Vm

   重新建立連線代理人透過復原站台上的 Vm 集區。 在此情況下，您必須指定確切的主控件，要在其中建立集區的 VM。

   集區的 VM 名稱必須是唯一的使用前置詞和後置詞。 如果 VM 名稱已經存在，指令碼將會失敗。 此外，如果主要端 Vm 的編號是從 1 到 5，復原站台編號會繼續從 6。

   ```powershell
   ipmo RemoteDesktop; 
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1} 
   ```
9. 容錯移轉群組 3-Web 存取和閘道伺服器 VM

復原計劃看起來像這樣：

![使用 RDS 部署集區桌面的嚴重損壞復原計畫](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>個人桌面 RDS 部署

針對個人桌面與 RDS 部署中，Vm 群組，以便在出現的順序，新增的手動步驟和指令碼。

1. 容錯移轉群組 1-RDS 連線代理人 VM
2. 群組 1 的手動動作-更新 DNS

   在提高權限的模式中執行 PowerShell，連線代理人 VM 上。 執行下列命令，並等候數分鐘，以確保 DNS 更新為新值：

   ```
   ipconfig /registerdns
   ```
3. 群組 1 的指令碼-新增虛擬化主機
      
   修改下列指令碼執行在雲端中的每個虛擬化主機。 通常您新增到連線代理人虛擬化主機之後，您需要重新啟動主機。 請確保主機不需要重新開機擱置中之前的指令碼執行，否則它將會失敗。

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. 容錯移轉群組 2-範本 VM
5. 群組 2 的指令碼 1-開啟範本 VM 關閉
   
   範本 VM 至次要站台復原時，將會啟動，但它已執行過 sysprep VM，並完全無法啟動。 也 RDS 要求 VM 必須從它建立集區的 VM 組態的關機。 因此，我們需要將它關閉。 如果您有單一 VMM 伺服器時，範本的 VM 名稱會是相同的主要和次要資料庫。 因此，使用 VM 識別碼所指定*內容*以下指令碼變數。 如果您有多個範本，請將它們全部關閉。

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. 容錯移轉群組 3-個人 Vm
7. 群組 3 的指令碼 1-移除現有的個人 Vm 並將其新增

   因此可以在次要網站上建立新的 Vm，則您可以移除連接訊息代理程式上的主要站台的個人 Vm。 您需要擷取 Vm 的指派，並將虛擬機器重新加入至連線代理人，與指派的雜湊。 只會從集合中移除個人的 Vm，並重新加入它們。 將匯出的個人桌面配置，並將它重新匯入至集合中。

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName CEODesktops; 
   Export-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 

   Foreach($vm in $desktops){
     Remove-RDVirtualDesktopFromCollection -CollectionName CEODesktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   
   Import-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 
   ```
8. 容錯移轉群組 3-Web 存取和閘道伺服器 VM

您的方案看起來像這樣： 

![個人桌面 RDS 部署嚴重損壞復原計畫](media/rds-asr-personal-desktops-drplan.png)
