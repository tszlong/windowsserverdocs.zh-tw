---
title: Windows Server 上安裝 HYPER-V 角色
description: 說明如何安裝 HYPER-V 使用伺服器管理員] 或 [Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 8e871317-09d2-4314-a6ec-ced12b7aee89
author: KBDAzure
ms.author: kathydav
ms.date: 12/02/2016
ms.openlocfilehash: a4167065c11cf87ba761ed65884b88c5413dfdf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870429"
---
# <a name="install-the-hyper-v-role-on-windows-server"></a>Windows Server 上安裝 HYPER-V 角色

>適用於：Windows Server 2016、windows Server 2019
  
若要建立並執行虛擬機器，安裝 HYPER-V 角色在 Windows Server 上使用伺服器管理員或**Install-windowsfeature**在 Windows PowerShell cmdlet。 適用於 Windows 10，請參閱[安裝 Hyper-v 角色在 Windows 10 上](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)。

若要深入了解 HYPER-V，請參閱[HYPER-V 技術概觀](..\Hyper-V-Technology-Overview.md)。 若要試用 Windows Server 2019，您可以下載並安裝評估版。 請參閱[評估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)。

您安裝 Windows Server，或新增 HYPER-V 角色之前，請確定：
- 您的電腦硬體都相容。 如需詳細資訊，請參閱 <<c0> [ 適用於 Windows Server 系統需求](../../../get-started/System-Requirements.md)並[適用於 Windows Server 上的 HYPER-V 系統需求](../System-requirements-for-Hyper-V-on-Windows.md)。
- 您不打算使用依賴相同 HYPER-V 需要的處理器功能的協力廠商虛擬化應用程式。 範例包括 VMWare Workstation 和 VirtualBox。 您可以安裝 HYPER-V，但不解除安裝這些應用程式。 但是，如果您嘗試使用它們來管理虛擬機器，在執行 HYPER-V hypervisor 時，虛擬機器可能無法啟動，或可能 unreliably 執行。 如需詳細資訊和指示關閉 HYPER-V hypervisor，如果您需要使用其中一個應用程式，請參閱[虛擬化應用程式無法運作搭配 HYPER-V、 Device Guard 和 Credential Guard](https://support.microsoft.com/help/3204980/virtualization-applications-do-not-work-together-with-hyper-v-device-g)。

如果您想要只安裝管理工具，例如 HYPER-V 管理員 中，請參閱[從遠端管理 HYPER-V 主機與 HYPER-V Manager](..\Manage\Remotely-manage-Hyper-V-hosts.md)。
  
## <a name="BKMK_SERV"></a>使用伺服器管理員安裝 HYPER-V  
  
1. 在 [伺服器管理員] 的 [管理] 功能表上，按一下 [新增角色及功能]。  
  
2. 在 [在您開始前] 頁面上，確認已準備好目的地伺服器和網路環境，以便安裝您要的角色和功能。 按一下 [下一步] 。  
  
3. 在 [選取安裝類型] 頁面上，選取 [角色型或功能型安裝]，然後按 [下一步]。  
  
4. 在 [選取目的地伺服器] 頁面上，從伺服器集區選取伺服器，然後按 [下一步]。  
  
5. 在 [選取伺服器角色] 頁面上，選取 [Hyper-V]。  
  
6. 若要新增用來建立和管理虛擬機器的工具，請按一下 [新增功能]。 在 [功能] 頁面上，按 [下一步]。  
  
7. 在 [建立虛擬交換器] 頁面、[虛擬機器移轉] 頁面和 [預設存放區] 頁面上，選取適當的選項。  
  
8. 在 [確認安裝選項] 頁面上，選取 [需要時自動重新啟動目的伺服器]，然後按一下 [安裝]。  
  
9. 當安裝完成時，請確認已正確安裝 HYPER-V。 開啟**所有伺服器**頁面在 [伺服器管理員] 中，並選取安裝了 HYPER-V 的伺服器。 請檢查**角色和功能**針對所選伺服器頁面上的圖格。  
  
## <a name="BKMK_PWRSH"></a>使用 Install-windowsfeature cmdlet 安裝 HYPER-V  
  
1. 在 Windows 桌面上，按一下 \[開始\] 按鈕，然後輸入 **Windows PowerShell** 名稱的任何一部分。  
  
2. 以滑鼠右鍵按一下 Windows PowerShell，然後選取**系統管理員身分執行**。  
  
3. 若要在您從遠端連線到伺服器上安裝 HYPER-V，請執行下列命令並取代`<computer_name>`與伺服器的名稱。  
  
    ```powershell
    Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart  
    ```  
  
    如果您在本機連線到伺服器，執行命令，但不含`-ComputerName <computer_name>`。  
  
4. 在伺服器重新啟動之後，您可以看到已安裝 HYPER-V 角色，並查看有哪些其他的角色和功能安裝方式是執行下列命令：  
  
    ```powershell
    Get-WindowsFeature -ComputerName <computer_name>  
    ```  
  
    如果您在本機連線到伺服器，執行命令，但不含`-ComputerName <computer_name>`。  
  
> [!NOTE]  
> 如果您在執行 Windows Server 2016 的 Server Core 安裝選項的伺服器上安裝此角色，並使用參數`-IncludeManagementTools`，只有 HYPER-V 模組適用於 Windows PowerShell 的安裝。 您可以使用 GUI 管理工具，HYPER-V 管理員 中，從遠端管理執行 Server Core 安裝的 HYPER-V 主機的另一部電腦上。 如需有關遠端連線的指示，請參閱[從遠端管理 HYPER-V 主機與 HYPER-V Manager](..\Manage\Remotely-manage-Hyper-V-hosts.md)。  
  
## <a name="see-also"></a>另請參閱  
  
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/Microsoft.Windows.ServerManager.Migration/Install-WindowsFeature)  
