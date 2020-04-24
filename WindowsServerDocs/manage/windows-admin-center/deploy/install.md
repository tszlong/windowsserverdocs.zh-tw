---
title: 安裝 Windows Admin Center
description: 如何在 Windows 電腦或伺服器上安裝 Windows Admin Center，讓多個使用者可以使用網頁瀏覽器來存取 Windows Admin Center。
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: cab128a3da9fa58c598cebcdf188058631c33977
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "75950002"
---
# <a name="install-windows-admin-center"></a>安裝 Windows Admin Center

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

本主題說明如何在 Windows 電腦或伺服器上安裝 Windows Admin Center，讓多個使用者可以使用網頁瀏覽器來存取 Windows Admin Center。

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [深入了解 Windows Admin Center](../overview.md) 或[立即下載](https://aka.ms/windowsadmincenter)。

## <a name="determine-your-installation-type"></a>判斷您的安裝類型

請參閱[安裝選項](../plan/installation-options.md)，其中包括[支援的作業系統](https://docs.microsoft.com/windows-server/manage/windows-admin-center/plan/installation-options#installation-supported-operating-systems)。 若要在 Azure 中的 VM 上安裝 Windows Admin Center，請參閱＜[在 Azure 中部署 Windows Admin Center](../azure/deploy-wac-in-azure.md)＞。

## <a name="install-on-windows-10"></a>安裝在 Windows 10 上

安裝在 Windows 10 時，Windows Admin Center 預設會使用連接埠 6516，但您可以選擇指定不同的連接埠。 您也可以建立桌面捷徑，並且讓 Windows Admin Center 管理您的 TrustedHosts。

> [!NOTE]
> 處於工作群組環境，或在網域中使用本機系統管理員認證時，需要修改 TrustedHosts。 如果您選擇省略這項設定，就必須[手動設定 TrustedHosts](../support/troubleshooting.md#configure-trustedhosts)。

當您從 [開始]  功能表啟動 Windows Admin Center 時，其會在預設瀏覽器中開啟。

第一次啟動 Windows Admin Center 時，您會在桌面的通知區域中看到圖示。 在此圖示上按一下滑鼠右鍵，然後選擇 [開啟]  在預設瀏覽器中開啟工具，或選擇 [結束]  以結束背景處理程序。

## <a name="install-on-windows-server-with-desktop-experience"></a>安裝在具有桌面體驗的 Windows Server

在 Windows Server 上，Windows Admin Center 會安裝為網路服務。 您必須指定服務接聽的連接埠，而且此服務需要 HTTPS 的憑證。 安裝程式可以建立自我簽署憑證以供測試，您也可以提供電腦上已安裝憑證的指紋。 如果您使用產生的憑證，此憑證將會與伺服器的 DNS 名稱相符。 如果您使用自己的憑證，請確定憑證中提供的名稱符合電腦名稱 (不支援萬用字元憑證)。您也可以選擇讓 Windows Admin Center 管理您的 TrustedHosts。

> [!NOTE]
> 處於工作群組環境，或在網域中使用本機系統管理員認證時，需要修改 TrustedHosts。 如果您選擇省略這項設定，就必須[手動設定 TrustedHosts](../support/troubleshooting.md#configure-trustedhosts)

安裝完成後，請從遠端電腦開啟瀏覽器，並瀏覽至安裝程式最後一個步驟中顯示的 URL。

> [!WARNING]
> 自動產生的憑證將於安裝後 60 天過期。

## <a name="install-on-server-core"></a>安裝在 Server Core

如果您有 Windows Server 的 Server Core 安裝，則可以從命令提示字元 (以系統管理員身分執行) 安裝 Windows Admin Center。 分別使用 `SME_PORT` 和 `SSL_CERTIFICATE_OPTION` 引數，指定連接埠和 SSL 憑證。 如果您要使用現有的憑證，請使用 `SME_THUMBPRINT` 來指定其指紋。

> [!WARNING]
> 安裝 Windows Admin Center 會重新啟動 WinRM 服務，這將會中斷所有遠端 PowerShells 工作階段。 建議您從本機 Cmd 或 PowerShell 安裝。 如果您要使用會在 WinRM 服務重新啟動時中斷的自動化解決方案進行安裝，您可以將參數 ```RESTART_WINRM=0``` 新增至安裝引數，但必須重新啟動 WinRM，Windows Admin Center 才會運作。

執行下列命令以安裝 Windows Admin Center，並自動產生自我簽署憑證：

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

執行下列命令以使用現有憑證來安裝 Windows Admin Center：

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> 不要使用點斜線相對路徑標記法 (例如，`.\<WindowsAdminCenterInstallerName>.msi`)，從 PowerShell 叫用 `msiexec`。 該標記法不受支援，而安裝將會失敗。 移除 `.\` 前置詞，或指定 MSI 的完整路徑。

## <a name="upgrading-to-a-new-version-of-windows-admin-center"></a>升級至新版本的 Windows Admin Center

您可以使用 Microsoft Update 或以手動安裝的方式更新非預覽版的 Windows Admin Center。

升級至新版本的 Windows Admin Center 時會保留您的設定。 我們並未正式支援升級 Windows Admin Center 的 Insider Preview 版本，因為我們認為最好執行全新安裝，但我們不會將其封鎖。

## <a name="updating-the-certificate-used-by-windows-admin-center"></a>更新 Windows Admin Center 所使用的憑證

當您將 Windows Admin Center 部署為服務時，必須提供 HTTPS 的憑證。 若要稍後再更新此憑證，請重新執行安裝程式，然後選擇 ```change```。
