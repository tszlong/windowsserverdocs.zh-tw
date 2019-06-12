---
title: 從遠端管理 HYPER-V 主機
description: 描述 HYPER-V 主機和 HYPER-V 管理員，以及如何連接到在不同的環境，包括跨網域和獨立的遠端主機之間的版本相容性。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
author: KBDAzure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: d4d9f2dd3727e196bb6893fd5041fa3f08c30796
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453185"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>從遠端管理 HYPER-V 主機與 HYPER-V 管理員

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10，Windows 8.1

這篇文章列出之支援的組合的 HYPER-V 主機和 HYPER-V 管理員版本，並說明如何連接至遠端和本機 HYPER-V 主機，因此您可以管理它們。 

HYPER-V 管理員可讓您管理少量的 HYPER-V 主機、 遠端和本機。 當您安裝 HYPER-V 管理工具，您可以安裝完整透過 HYPER-V 安裝或僅限工具安裝。 僅限工具安裝表示您可以使用這些工具不符合 HYPER-V 主機的硬體需求的電腦上。 如需 HYPER-V 主機的硬體的詳細資訊，請參閱[系統需求](../System-requirements-for-Hyper-V-on-Windows.md)。

如果未安裝 HYPER-V 管理員，請參閱[指示](#install-hyper-v-manager)如下。

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>支援的 HYPER-V Manager 和 HYPER-V 主機版本的組合

在某些情況下您可以在主機上，使用不同版本的 HYPER-V 管理員比 HYPER-V 版本，在資料表中所示。 當您這樣做時，HYPER-V 管理員會提供您管理在主機上的適用於 Hyper-v 的安全版本的功能。 例如，如果您使用 HYPER-V 管理員的新版 Windows Server 2012 R2 中遠端管理 Windows Server 2012 中執行 HYPER-V 的主機，您無法使用該 HYPER-V 主機上的 Windows Server 2012 R2 中可用的功能。

下表顯示從特定版本的 HYPER-V 管理員可以管理哪些版本的 HYPER-V 主機。 僅支援會列出版本的作業系統。 如需特定的作業系統版本的支援狀態的詳細資訊，請使用**搜尋產品生命週期**按鈕[Microsoft 週期原則](https://support.microsoft.com/lifecycle)頁面。 一般情況下，較舊版本的 HYPER-V 管理員只能管理執行相同版本或類似的 Windows Server 版本的 HYPER-V 主機。

|HYPER-V 管理員版本 | HYPER-V 主機版本|
|---|---|
|Windows 2016 中，Windows 10|Windows Server 2016-所有版本和安裝都選項，包括 Nano 伺服器，以及對應版本的 HYPER-V 伺服器 <br>Windows Server 2012 R2-所有版本和安裝選項，以及對應版本的 HYPER-V 伺服器 <br>Windows Server 2012，所有版本和安裝選項，以及對應版本的 HYPER-V 伺服器 <br> - Windows 10 <br> - Windows 8.1  |
| Windows Server 2012 R2, Windows 8.1 | Windows Server 2012 R2-所有版本和安裝選項，以及對應版本的 HYPER-V 伺服器 <br>Windows Server 2012，所有版本和安裝選項，以及對應版本的 HYPER-V 伺服器 <br>- Windows 8.1
| Windows Server 2012 | Windows Server 2012，所有版本和安裝選項，以及對應版本的 HYPER-V 伺服器
| Windows Server 2008 R2 Service Pack 1、 Windows 7 Service Pack 1 | Windows Server 2008 R2-所有版本和安裝選項，以及對應版本的 HYPER-V 伺服器
| Windows Server 2008 的 Windows Vista Service Pack 2 | Windows Server 2008-所有版本和安裝選項，以及對應版本的 HYPER-V 伺服器

>[!NOTE]
>適用於 Windows 8 的 Service pack 支援是在 2016 年 1 月 12 日結束。 如需詳細資訊，請參閱 < [Windows 8.1 常見問題集](https://support.microsoft.com/help/18581)。

## <a name="connect-to-a-hyper-v-host"></a>連線到 HYPER-V 主機

若要從 HYPER-V 管理員，以連接至 HYPER-V 主機，以滑鼠右鍵按一下**HYPER-V 管理員**中左的窗格中，然後按一下**連接到伺服器**。

## <a name="manage-hyper-v-on-a-local-computer"></a>管理本機電腦上的 HYPER-V

HYPER-V 管理員中未列出任何裝載 HYPER-V，直到您將新增的電腦，包括本機電腦的電腦。 請這樣做：

1. 在左窗格中，以滑鼠右鍵按一下**HYPER-V 管理員**。
2. 按一下 **連接到伺服器**。
3. 從**選取的電腦**，按一下**本機電腦**，然後按一下 **確定**。

如果您無法連線：

* 很可能只有 HYPER-V 工具確認已安裝。 若要檢查已安裝的 HYPER-V 平台，請尋找虛擬機器管理服務。 / (開啟服務的傳統型應用程式： 按一下**開始**，按一下**開始搜尋**方塊中，輸入**services.msc**，然後按**Enter**。 如果未列出的虛擬機器管理服務，請依照下列中的指示安裝 HYPER-V 平台[安裝 Hyper-v 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。
* 請確認您的硬體符合需求。 請參閱[系統需求](../System-requirements-for-Hyper-V-on-Windows.md)。
* 請檢查您的使用者帳戶屬於 Administrators 群組或 HYPER-V Administrators 群組。

## <a name="manage-hyper-v-hosts-remotely"></a>從遠端管理 HYPER-V 主機  

若要管理遠端 HYPER-V 主機，啟用遠端管理本機電腦和遠端主機上。

在 Windows 伺服器上，開啟 伺服器管理員\>**本機伺服器** \>**遠端管理**，然後按一下 **允許遠端連線到此電腦**. 

或者，從任一作業系統中，開啟 Windows PowerShell，以系統管理員身分，然後執行： 

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>連線到相同的網域中的主機

Windows 8.1 及更早版本，只有當主機是在相同的網域，而且您的本機使用者帳戶也是在遠端主機上時，適用於遠端管理。

若要將遠端 HYPER-V 主機加入 HYPER-V 管理員，請選取**另一部電腦**中**選取電腦**對話方塊，然後輸入遠端主機的主機名稱、 NetBIOS 名稱或完整的網域名稱\(FQDN\)。

在 Windows Server 2016 和 Windows 10 中的 HYPER-V 管理員提供更多類型的遠端連線，比早期版本，下列各節所述。  

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>不同的使用者身分連線到 Windows 2016 或 Windows 10 的遠端主機

這可讓您連線到 HYPER-V 主機，當您未執行時在本機電腦上為該使用者是 HYPER-V Administrators 群組或 HYPER-V 主機上的 Administrators 群組的成員。 請這樣做：

1. 在左窗格中，以滑鼠右鍵按一下**HYPER-V 管理員**。
1. 按一下 **連接到伺服器**。
1. 選取 [**連線為其他使用者**中**選取電腦**] 對話方塊。
1. 選取 **設定使用者**。

>[!NOTE]
> 這只適用於 Windows Server 2016 或 Windows 10**遠端**主機。

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>連接至 Windows 2016 或 Windows 10 的遠端主機使用的 IP 位址

請這樣做：

1. 在左窗格中，以滑鼠右鍵按一下**HYPER-V 管理員**。
1. 按一下 **連接到伺服器**。
1. 輸入 IP 位址**另一部電腦**文字欄位。

>[!NOTE]
> 這只適用於 Windows Server 2016 或 Windows 10**遠端**主機。

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>連接至 Windows 2016 或 Windows 10 的遠端主機超出您的網域，或沒有網域

請這樣做：

1. 在受管理 HYPER-V 主機上，開啟 Windows PowerShell 工作階段，以系統管理員身分。

1. 建立必要的防火牆規則的私人網路區域：   
   
   ```
   Enable-PSRemoting
   ```

2. 若要允許在公用區域上的遠端存取，啟用 CredSSP 和 WinRM 的防火牆規則：
  
   ```
   Enable-WSManCredSSP -Role server
   ```

    如需詳細資訊，請參閱 < [Enable-psremoting](https://technet.microsoft.com/library/hh849694.aspx)並[Enable-wsmancredssp](https://technet.microsoft.com/library/hh849872.aspx)。

接下來，設定的電腦，您將使用來管理 HYPER-V 主機。

1. 開啟 Windows PowerShell 工作階段，以系統管理員身分。
1. 執行下列命令：

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. 此外，您可能也需要設定下列群組原則： 
    * **電腦設定** \> **系統管理範本** \> **系統** \> **認證委派** \> **允許委派新認證，使用僅限 NTLM 伺服器驗證**
    * 按一下 **啟用**並加入*wsman/fqdn 的-hyper-v-主機*。
1. 開啟**HYPER-V 管理員**。
1. 在左窗格中，以滑鼠右鍵按一下**HYPER-V 管理員**。
1. 按一下 **連接到伺服器**。

>[!NOTE]
> 這只適用於 Windows Server 2016 或 Windows 10**遠端**主機。

如 cmdlet 的詳細資訊，請參閱 < [Set-item](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/set-item)並[Enable-wsmancredssp](https://technet.microsoft.com/library/hh849872.aspx)。

## <a name="install-hyper-v-manager"></a>安裝 HYPER-V 管理員

若要使用的 UI 工具，選擇適用於在您要在其中執行 HYPER-V 管理員的電腦上的作業系統：

在 Windows 伺服器上，開啟 伺服器管理員\>**管理** \> **新增角色及功能**。 移至**功能**頁面上，展開**遠端伺服器管理工具** \> **角色管理工具** \> **HYPER-V 管理工具**。 

在 Windows 中，HYPER-V 管理員可用於[任何包含 HYPER-V 的 Windows 作業系統](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility)。

1. 在 Windows 桌面上，按一下 [開始] 按鈕，然後開始輸入**程式和功能**。 
1. 在搜尋結果中，按一下**程式和功能**。
1. 在左窗格中，按一下**開啟或關閉開啟的 Windows 功能**。
1. 展開 HYPER-V 資料夾中，並**按一下 HYPER-V 管理工具**。
1. 若要安裝 HYPER-V 管理員，請按一下**HYPER-V 管理工具**。 如果您想要同時安裝 HYPER-V 模組，請按一下該選項。

若要使用 Windows PowerShell，請以系統管理員身分執行下列命令：

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="see-also"></a>另請參閱  
 
[安裝 Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md) 

