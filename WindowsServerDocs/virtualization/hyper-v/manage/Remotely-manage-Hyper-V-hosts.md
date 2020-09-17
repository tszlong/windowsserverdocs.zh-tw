---
title: 從遠端系統管理 Hyper-v 主機
description: 描述 Hyper-v 主機和 Hyper-v 管理員之間的版本相容性，以及如何連線至不同環境中的遠端主機，包括跨網域和獨立環境。
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
ms.author: benarm
author: BenjaminArmstrong
ms.date: 12/06/2016
ms.openlocfilehash: 2b0a7c93f5a6c6be7340c8d5b0a5bd93d21f1cec
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746653"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>使用 Hyper-V 管理員管理遠端 Hyper-V 主機

>適用于： Windows Server 2016、Windows Server 2012 R2、Windows 10、Windows 8。1

本文列出支援的 Hyper-v 主機和 Hyper-v 管理員版本組合，並說明如何連線到遠端和本機 Hyper-v 主機，讓您可以管理它們。

Hyper-v 管理員可讓您管理少數 Hyper-v 主機（遠端和本機）。 它是在您安裝 Hyper-v 管理工具時安裝的，您可以透過完整的 Hyper-v 安裝或僅限工具安裝來執行此作業。 進行僅限工具安裝，表示您可以在不符合硬體需求的電腦上使用這些工具來裝載 Hyper-v。 如需 Hyper-v 主機硬體的詳細資訊，請參閱 [系統需求](../System-requirements-for-Hyper-V-on-Windows.md)。

如果未安裝 Hyper-v 管理員，請參閱下列 [指示](#install-hyper-v-manager) 。

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>支援的 Hyper-v 管理員和 Hyper-v 主機版本組合

在某些情況下，您可以使用不同于主機上 hyper-v 版本的 Hyper-v 管理員版本，如下表所示。 當您這樣做時，Hyper-v 管理員會在您所管理的主機上提供適用于 Hyper-v 版本的功能。 例如，如果您使用 Windows Server 2012 R2 中的 Hyper-v 管理員版本從遠端系統管理在 Windows Server 2012 中執行 Hyper-v 的主機，您將無法在該 Hyper-v 主機上使用 Windows Server 2012 R2 中的可用功能。

下表顯示您可以從特定的 Hyper-v 管理員版本管理的 Hyper-v 主機版本。 只會列出支援的作業系統版本。 如需特定作業系統版本之支援狀態的詳細資訊，請使用[Microsoft 生命週期原則](https://support.microsoft.com/lifecycle)頁面上的 [**搜尋產品生命週期**] 按鈕。 一般而言，較舊版本的 Hyper-v 管理員只能管理執行相同版本或類似 Windows Server 版本的 Hyper-v 主機。

|Hyper-v 管理員版本 | Hyper-v 主機版本|
|---|---|
|Windows 2016，Windows 10|-Windows Server 2016-所有版本和安裝選項，包括 Nano Server 以及對應版本的 Hyper-v 伺服器 <br>-Windows Server 2012 R2-所有版本與安裝選項，以及對應版本的 Hyper-v 伺服器 <br>-Windows Server 2012 —所有版本與安裝選項，以及對應版本的 Hyper-v 伺服器 <br> - Windows 10 <br> - Windows 8.1  |
| Windows Server 2012 R2、Windows 8。1 | -Windows Server 2012 R2-所有版本與安裝選項，以及對應版本的 Hyper-v 伺服器 <br>-Windows Server 2012 —所有版本與安裝選項，以及對應版本的 Hyper-v 伺服器 <br>- Windows 8.1
| Windows Server 2012 | -Windows Server 2012 —所有版本與安裝選項，以及對應版本的 Hyper-v 伺服器
| Windows Server 2008 R2 Service Pack 1、Windows 7 Service Pack 1 | -Windows Server 2008 R2-所有版本與安裝選項，以及對應版本的 Hyper-v 伺服器
| Windows Server 2008、Windows Vista Service Pack 2 | -Windows Server 2008-所有版本與安裝選項，以及對應版本的 Hyper-v 伺服器

>[!NOTE]
>Service pack 支援于2016年1月12日結束 Windows 8。 如需詳細資訊，請參閱 [WINDOWS 8.1 常見問題](https://support.microsoft.com/help/18581)。

## <a name="connect-to-a-hyper-v-host"></a>連接至 Hyper-v 主機

若要從 Hyper-v 管理員連線到 Hyper-v 主機，請在左窗格中的 [ **Hyper-v 管理員** ] 上按一下滑鼠右鍵，然後按一下 [連線 **到伺服器]**。

## <a name="manage-hyper-v-on-a-local-computer"></a>管理本機電腦上的 Hyper-v

在您新增電腦之前（包括本機電腦），hyper-v 管理員不會列出任何裝載 Hyper-v 的電腦。 若要這樣做：

1. 在左窗格中，以滑鼠右鍵按一下 [ **Hyper-v 管理員**]。
2. 按一下 **[連接到伺服器]**。
3. 在 [ **選取電腦**] 中，按一下 [ **本機電腦** ]，然後按一下 **[確定]**。

如果您無法連接：

* 可能只會安裝 Hyper-v 工具。 若要檢查是否已安裝 Hyper-v 平臺，請尋找虛擬機器管理服務。 / (開啟服務桌面應用程式：按一下 [ **開始**]，按一下 [ **開始搜尋** ] 方塊，輸入 **services.msc**，然後按 **enter**。 如果未列出虛擬機器管理服務，請遵循 [安裝 hyper-v](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)中的指示安裝 hyper-v 平臺。
* 檢查您的硬體是否符合需求。 請參閱 [系統需求](../System-requirements-for-Hyper-V-on-Windows.md)。
* 確認您的使用者帳戶屬於「系統管理員」群組或「Hyper-v 系統管理員」群組。

## <a name="manage-hyper-v-hosts-remotely"></a>從遠端系統管理 Hyper-v 主機

若要管理遠端 Hyper-v 主機，請在本機電腦和遠端主機上啟用遠端系統管理。

在 Windows Server 上，開啟伺服器管理員 \> **本機伺服器** \> **遠端系統管理**]，然後按一下 [**允許遠端連線到這部電腦**]。

或者，從任一作業系統，以系統管理員身分開啟 Windows PowerShell，然後執行：

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>連接至相同網域中的主機

針對 Windows 8.1 及更早版本，只有當主機位於相同網域，且您的本機使用者帳戶也在遠端主機上時，遠端系統管理才能運作。

若要將遠端 Hyper-v 主機新增至 Hyper-v 管理員，請在 [**選取電腦**] 對話方塊中選取**另一部電腦**，然後輸入遠端主機的主機名稱、NetBIOS 名稱或完整功能變數名稱稱 \( FQDN \) 。

Windows Server 2016 和 Windows 10 中的 hyper-v 管理員提供比舊版更多的遠端連線類型，如下列各節所述。

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>以不同的使用者連接到 Windows 2016 或 Windows 10 遠端主機

當您不是在本機電腦上以 hyper-v 系統管理員群組或 Hyper-v 主機上的 Administrators 群組成員的使用者身分執行時，這可讓您連線到 Hyper-v 主機。 若要這樣做：

1. 在左窗格中，以滑鼠右鍵按一下 [ **Hyper-v 管理員**]。
1. 按一下 **[連接到伺服器]**。
1. 在 [**選取電腦**] 對話方塊中選取 **[以其他使用者的身份連線]** 。
1. 選取 [ **設定使用者**]。

>[!NOTE]
> 這僅適用于 Windows Server 2016 或 Windows 10 **遠端** 主機。

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>使用 IP 位址連接到 Windows 2016 或 Windows 10 遠端主機

若要這樣做：

1. 在左窗格中，以滑鼠右鍵按一下 [ **Hyper-v 管理員**]。
1. 按一下 **[連接到伺服器]**。
1. 在 [ **另一台電腦** ] 文字欄位中輸入 IP 位址。

>[!NOTE]
> 這僅適用于 Windows Server 2016 或 Windows 10 **遠端** 主機。

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>連線到 Windows 2016 或 Windows 10 您網域以外的遠端主機，或不含網域的遠端主機

若要這樣做：

1. 在要管理的 Hyper-v 主機上，以系統管理員身分開啟 Windows PowerShell 會話。

1. 針對私人網路區域建立必要的防火牆規則：

   ```
   Enable-PSRemoting
   ```

2. 若要允許在公用區域上進行遠端存取，請啟用 CredSSP 和 WinRM 的防火牆規則：

   ```
   Enable-WSManCredSSP -Role server
   ```

    如需詳細資訊，請參閱 [啟用->enable-psremoting](/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-7) 和 [啟用-WSManCredSSP](/powershell/module/microsoft.wsman.management/enable-wsmancredssp?view=powershell-7)。

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
    * **電腦設定** \>**系統管理範本** \>**系統** \>**認證委派** \>**允許使用僅限 NTLM 伺服器驗證委派全新的認證**
    * 按一下 [ **啟用** ]，然後新增 [ *wsman/fqdn-hyper-v-主機*]。
1. 開啟 [ **Hyper-v 管理員**]。
1. 在左窗格中，以滑鼠右鍵按一下 [ **Hyper-v 管理員**]。
1. 按一下 **[連接到伺服器]**。

>[!NOTE]
> 這僅適用于 Windows Server 2016 或 Windows 10 **遠端** 主機。

如需 Cmdlet 的詳細資訊，請參閱 [Set-Item](/powershell/module/microsoft.powershell.management/set-item?view=powershell-7) 和 [Enable-WSManCredSSP](/powershell/module/microsoft.wsman.management/enable-wsmancredssp?view=powershell-7)。

## <a name="install-hyper-v-manager"></a>安裝 Hyper-v 管理員

若要使用 UI 工具，請在您要執行 Hyper-v 管理員的電腦上，選擇適用于作業系統的工具：

在 Windows Server 上，開啟伺服器管理員 \> **管理** \> **新增角色及功能**]。 移至 [ **功能** ] 頁面，然後展開 [ **遠端伺服器管理工具** \> **角色管理工具**] \> **hyper-v 管理**工具。

在 Windows 中，Hyper-v 管理員可在 [任何包含 hyper-v 的 windows 作業系統](/virtualization/hyper-v-on-windows/reference/hyper-v-requirements)上使用。

1. 在 Windows 桌面上，按一下 [[開始] 按鈕]，然後開始輸入 [ **程式和功能**]。
1. 在 [搜尋結果] 中，按一下 [ **程式和功能**]。
1. 在左窗格中，按一下 [ **開啟或關閉 Windows 功能**]。
1. 展開 [Hyper-v] 資料夾，然後 **按一下 [Hyper-v 管理工具**]。
1. 若要安裝 Hyper-v 管理員，請按一下 [ **Hyper-v 管理工具**]。 如果您也要安裝 Hyper-v 模組，請按一下該選項。

若要使用 Windows PowerShell，請以系統管理員身分執行下列命令：

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="additional-references"></a>其他參考資料

[安裝 Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)