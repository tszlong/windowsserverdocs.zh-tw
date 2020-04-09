---
title: 從遠端系統管理 Hyper-v 主機
description: 描述 Hyper-v 主機與 Hyper-v 管理員之間的版本相容性，以及如何連接到不同環境（包括跨網域和獨立）中的遠端主機。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
author: kbdazure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: 592bb6352c4ca56770e1a3051ecbc88d9d378467
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859411"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>使用 Hyper-v 管理員從遠端系統管理 Hyper-v 主機

>適用于： Windows Server 2016、Windows Server 2012 R2、Windows 10 Windows 8。1

本文列出支援的 Hyper-v 主機和 Hyper-v 管理員版本組合，並說明如何連線到遠端和本機 Hyper-v 主機，以便您進行管理。 

Hyper-v 管理員可讓您管理少量的 Hyper-v 主機，包括遠端和本機。 它是在您安裝 Hyper-v 管理工具時安裝的，您可以透過完整的 Hyper-v 安裝或僅限工具安裝來執行。 執行僅限工具安裝表示您可以在不符合硬體需求的電腦上使用這些工具來裝載 Hyper-v。 如需 Hyper-v 主機硬體的詳細資訊，請參閱[系統需求](../System-requirements-for-Hyper-V-on-Windows.md)。

如果未安裝 Hyper-v 管理員，請參閱下列[指示](#install-hyper-v-manager)。

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>支援的 Hyper-v 管理員和 Hyper-v 主機版本組合

在某些情況下，您可以使用不同版本的 Hyper-v 管理員，而不是主機上的 Hyper-v 版本，如下表所示。 當您這麼做時，Hyper-v 管理員會提供您所管理之主機上的 Hyper-v 版本可用的功能。 例如，如果您使用 Windows Server 2012 R2 中的 Hyper-v 管理員版本，從遠端系統管理在 Windows Server 2012 中執行 Hyper-v 的主機，您將無法在該 Hyper-v 主機上使用 Windows Server 2012 R2 中提供的功能。

下表顯示您可以從特定版本的 Hyper-v 管理員管理哪些 Hyper-v 主機版本。 只會列出支援的作業系統版本。 如需特定作業系統版本之支援狀態的詳細資訊，請使用[Microsoft 生命週期原則](https://support.microsoft.com/lifecycle)頁面上的 [**搜尋產品週期**] 按鈕。 一般來說，舊版的 Hyper-v 管理員只能管理執行相同版本或可比較 Windows Server 版本的 Hyper-v 主機。

|Hyper-v 管理員版本 | Hyper-v 主機版本|
|---|---|
|Windows 2016、Windows 10|-Windows Server 2016 —所有版本和安裝選項，包括 Nano Server，以及對應的 Hyper-v 伺服器版本 <br>-Windows Server 2012 R2 —所有版本和安裝選項，以及對應版本的 Hyper-v 伺服器 <br>-Windows Server 2012 —所有版本和安裝選項，以及對應版本的 Hyper-v 伺服器 <br> -Windows 10 <br> -Windows 8。1  |
| Windows Server 2012 R2、Windows 8.1 | -Windows Server 2012 R2 —所有版本和安裝選項，以及對應版本的 Hyper-v 伺服器 <br>-Windows Server 2012 —所有版本和安裝選項，以及對應版本的 Hyper-v 伺服器 <br>-Windows 8。1
| Windows Server 2012 | -Windows Server 2012 —所有版本和安裝選項，以及對應版本的 Hyper-v 伺服器
| Windows Server 2008 R2 Service Pack 1、Windows 7 Service Pack 1 | -Windows Server 2008 R2 —所有版本和安裝選項，以及對應版本的 Hyper-v 伺服器
| Windows Server 2008、Windows Vista Service Pack 2 | -Windows Server 2008 —所有版本和安裝選項，以及對應版本的 Hyper-v 伺服器

>[!NOTE]
>Service pack 支援于2016年1月12日結束 Windows 8。 如需詳細資訊，請參閱[WINDOWS 8.1 常見問題](https://support.microsoft.com/help/18581)。

## <a name="connect-to-a-hyper-v-host"></a>連接至 Hyper-v 主機

若要從 Hyper-v 管理員連線到 Hyper-v 主機，請在左窗格中的 [ **Hyper-v 管理員**] 上按一下滑鼠右鍵，然後按一下 [連線**到伺服器]** 。

## <a name="manage-hyper-v-on-a-local-computer"></a>管理本機電腦上的 Hyper-v

Hyper-v 管理員不會列出任何裝載 Hyper-v 的電腦，除非您新增電腦，包括本機電腦。 若要這樣做：

1. 在左窗格中，以滑鼠右鍵按一下 [ **Hyper-v 管理員**]。
2. 按一下 **[連接到伺服器]** 。
3. 在 [**選取電腦**] 中，按一下 [**本機電腦**]，然後按一下 **[確定]** 。

如果您無法連接：

* 可能只會安裝 Hyper-v 工具。 若要確認是否已安裝 Hyper-v 平臺，請尋找虛擬機器管理服務。 /（開啟 [服務] 桌面應用程式：按一下 [**開始**]，按一下 [**開始搜尋**] 方塊，輸入**services.msc**，然後按**enter**鍵。 如果未列出虛擬機器管理服務，請遵循[安裝 hyper-v](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)中的指示來安裝 hyper-v 平臺。
* 檢查您的硬體是否符合需求。 請參閱[系統需求](../System-requirements-for-Hyper-V-on-Windows.md)。
* 檢查您的使用者帳戶是否屬於 Administrators 群組或 Hyper-v Administrators 群組。

## <a name="manage-hyper-v-hosts-remotely"></a>遠端系統管理 Hyper-v 主機  

若要管理遠端 Hyper-v 主機，請啟用本機電腦和遠端主機上的遠端系統管理。

在 Windows Server 上，開啟伺服器管理員 \>**本機伺服器**\>**遠端系統管理**，然後按一下 **允許遠端連線到這部電腦**。 

或者，在任一作業系統上，以系統管理員身分開啟 Windows PowerShell 並執行： 

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>連接至相同網域中的主機

針對 Windows 8.1 和更早版本，只有當主機位於相同網域，而且您的本機使用者帳戶也在遠端主機上時，遠端系統管理才有作用。

若要將遠端 Hyper-v 主機新增至 Hyper-v 管理員，請在 [**選取電腦**] 對話方塊中選取 [**另一台電腦**]，然後輸入遠端主機的主機名稱、NetBIOS 名稱或完整功能變數名稱 \(FQDN\)。

Windows Server 2016 和 Windows 10 中的 hyper-v 管理員提供比先前版本更多的遠端連線類型，如下節所述。  

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>以不同的使用者身分連接至 Windows 2016 或 Windows 10 遠端主機

當您不是在本機電腦上以 hyper-v 系統管理員群組或 Hyper-v 主機上的 Administrators 群組成員的使用者身分執行時，這可讓您連線至 Hyper-v 主機。 若要這樣做：

1. 在左窗格中，以滑鼠右鍵按一下 [ **Hyper-v 管理員**]。
1. 按一下 **[連接到伺服器]** 。
1. 在 [**選取電腦**] 對話方塊中選取 **[以其他使用者**身分連線]。
1. 選取 [**設定使用者**]。

>[!NOTE]
> 這只適用于 Windows Server 2016 或 Windows 10**遠端**主機。

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>使用 IP 位址連接到 Windows 2016 或 Windows 10 遠端主機

若要這樣做：

1. 在左窗格中，以滑鼠右鍵按一下 [ **Hyper-v 管理員**]。
1. 按一下 **[連接到伺服器]** 。
1. 在 [**另一台電腦**] 文字欄位中輸入 IP 位址。

>[!NOTE]
> 這只適用于 Windows Server 2016 或 Windows 10**遠端**主機。

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>連線到網域外的 Windows 2016 或 Windows 10 遠端主機，或不含網域的連線

若要這樣做：

1. 在要管理的 Hyper-v 主機上，以系統管理員身分開啟 Windows PowerShell 會話。

1. 為私人網路區域建立必要的防火牆規則：   
   
   ```
   Enable-PSRemoting
   ```

2. 若要允許在公用區域上進行遠端存取，請啟用 CredSSP 和 WinRM 的防火牆規則：
  
   ```
   Enable-WSManCredSSP -Role server
   ```

    如需詳細資訊，請參閱[enable-PSRemoting](https://technet.microsoft.com/library/hh849694.aspx) and [enable-enable-wsmancredssp](https://technet.microsoft.com/library/hh849872.aspx)。

接下來，設定您要用來管理 Hyper-v 主機的電腦。

1. 以系統管理員身分開啟 Windows PowerShell 會話。
1. 執行下列命令：

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. 您可能也需要設定下列群組原則： 
    * **電腦**設定 \>**系統管理範本**\>**系統**\>**認證委派**\>**允許使用僅限 NTLM 伺服器驗證來委派新認證**
    * 按一下 [**啟用**]，然後新增 [ *wsman/fqdn-hyper-v 主機*]。
1. 開啟 [ **Hyper-v 管理員**]。
1. 在左窗格中，以滑鼠右鍵按一下 [ **Hyper-v 管理員**]。
1. 按一下 **[連接到伺服器]** 。

>[!NOTE]
> 這只適用于 Windows Server 2016 或 Windows 10**遠端**主機。

如需 Cmdlet 的詳細資訊，請參閱[Set-Item](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/set-item)和[Enable-enable-wsmancredssp](https://technet.microsoft.com/library/hh849872.aspx)。

## <a name="install-hyper-v-manager"></a>安裝 Hyper-v 管理員

若要使用 UI 工具，請在您將執行 Hyper-v 管理員的電腦上，選擇適合作業系統的程式：

在 Windows Server 上，開啟伺服器管理員 \>**管理**\>**新增角色及功能**。 移至 **功能** 頁面，並展開 **遠端伺服器管理工具** \> **角色管理**工具 \> **hyper-v 管理**工具。 

在 Windows 上，Hyper-v 管理員可在[包含 hyper-v 的任何 Windows 作業系統](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility)上使用。

1. 在 Windows 桌面上，按一下 [開始] 按鈕，然後開始鍵入 [**程式和功能**]。 
1. 在 [搜尋結果] 中，按一下 [**程式和功能**]。
1. 在左窗格中，按一下 [**開啟或關閉 Windows 功能**]。
1. 展開 [Hyper-v] 資料夾，然後**按一下 [Hyper-v 管理工具**]。
1. 若要安裝 Hyper-v 管理員，請按一下 [ **Hyper-v 管理工具**]。 如果您也想要安裝 Hyper-v 模組，請按一下該選項。

若要使用 Windows PowerShell，請以系統管理員身分執行下列命令：

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="see-also"></a>另請參閱  
 
[安裝 Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md) 

