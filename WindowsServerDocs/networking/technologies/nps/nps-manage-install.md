---
title: 安裝網路原則伺服器
description: 您可以使用本主題使用來安裝網路原則伺服器 (NPS) 是 Windows PowerShell 或 [新增角色及功能精靈] 在 Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9d11f77ff8b9db8bc10970b325afae60ba937cfa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831189"
---
# <a name="install-network-policy-server"></a>安裝網路原則伺服器

您可以使用本主題使用來安裝網路原則伺服器 (NPS) 是 Windows PowerShell 或 [新增角色及功能精靈]。 NPS 是網路原則與存取服務伺服器角色的角色服務。

> [!NOTE]
> 根據預設值，NPS 會為所有已安裝的網路介面卡接聽連接埠 1812、1813、1645 和 1646 上的 RADIUS 流量。 如果您安裝 NPS 時啟用具有進階安全性的 Windows 防火牆時，這些連接埠的防火牆例外狀況會自動建立這兩個網際網路通訊協定第 6 版的安裝程序期間\(IPv6\)與 IPv4 流量。 如果您的網路存取伺服器會設定為透過非預設的連接埠傳送 RADIUS 流量，移除 NPS 在安裝期間，建立具有進階安全性的 Windows 防火牆中的例外狀況，並建立您要針對使用的連接埠的例外狀況RADIUS 流量。

**系統管理認證**

您必須是 **Domain Admins** 群組的成員，才可以完成這個程序。

## <a name="to-install-nps-by-using-windows-powershell"></a>若要使用 Windows PowerShell 安裝 NPS

若要使用 Windows PowerShell 中，系統管理員，以執行 Windows PowerShell 執行此程序輸入下列命令，並再按 ENTER 鍵。

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>若要使用伺服器管理員安裝 NPS

1.  在 NPS1 的 [伺服器管理員] 中，按一下 [管理]，然後按一下 [新增角色及功能]。 [新增角色及功能精靈] 隨即開啟。

2.  在 [在您開始前] 中，按 [下一步]。

    > [!NOTE]
    > 如果您先前在執行 [新增角色及功能精靈] 時已經選取 [預設略過這個頁面]，就不會顯示 [新增角色及功能精靈] 的 [在您開始前] 頁面。

3.  在 [選取安裝類型] 中，確定已選取 [角色型或功能型安裝]，然後按 [下一步]。

4.  在 [選取目的地伺服器] 中，確定已選取 [從伺服器集區選取伺服器]。 在 [伺服器集區] 中，確定已選取本機電腦。 按一下 [下一步] 。

5.  在 **選取伺服器角色**，請在**角色**，選取**網路原則與存取服務**。 會開啟一個對話方塊，要求是否網域控制站應該加入所需的網路原則與存取服務的功能。 按一下 **將功能加入**，然後按一下 **下一步**

6.  在 [選取功能] 中，按 [下一步]，然後在 [網路原則與存取服務]中，檢閱提供的資訊，接著按 [下一步]。

7.  在 [選取角色服務] 中，按一下 [網路原則伺服器]。  在 [新增網路原則伺服器所需的功能] 中，按一下 [新增功能]。 按一下 [下一步] 。

8.  在 [確認安裝選項] 中，按一下 [必要時自動重新啟動目的地伺服器]。 提示您確認這個選取項目時，按一下 [是]，然後按一下 [安裝]。 [安裝進度] 頁面會在安裝程序期間顯示狀態。 此程序完成時，訊息 」 上安裝成功*ComputerName*"的訊息，其中*ComputerName*是您安裝網路原則伺服器的電腦名稱。 按一下 **關閉**。

如需詳細資訊，請參閱 <<c0> [ 管理 NPSs](nps-manage-servers.md)。