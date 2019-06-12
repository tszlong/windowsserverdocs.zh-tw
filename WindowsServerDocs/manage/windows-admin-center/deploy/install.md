---
title: 安裝 Windows Admin Center
description: 如何安裝 Windows Admin Center，在 Windows 電腦上或在伺服器上，讓多個使用者可以存取 Windows Admin Center 使用網頁瀏覽器。
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a9eb7944cd35dfa68e3c36cdc6c016f483a9f1e1
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811961"
---
# <a name="install-windows-admin-center"></a>安裝 Windows Admin Center

> 適用於：Windows Admin Center，Windows Admin Center 預覽

本主題描述如何安裝 Windows Admin Center，在 Windows 電腦上或在伺服器上，讓多個使用者可以存取 Windows Admin Center 使用網頁瀏覽器。

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [深入了解 Windows Admin Center](../understand/windows-admin-center.md) 或[立即下載](https://aka.ms/windowsadmincenter).

## <a name="determine-your-installation-type"></a>判斷您的安裝類型

檢閱[安裝選項](../plan/installation-options.md)hvsocket.h[支援的作業系統](../plan/installation-options.md#supported-operating-systems-installation)。 若要在 Azure 中的 VM 上安裝 Windows Admin Center，請參閱[在 Azure 中部署 Windows Admin Center](../azure/deploy-wac-in-azure.md)。

## <a name="install-on-windows-10"></a>安裝在 Windows 10 上

安裝在 Windows 10 時，Windows Admin Center 預設會使用連接埠 6516，但您可以選擇定不同的連接埠。 您也可以建立桌面捷徑，並且讓 Windows Admin Center 管理您的 TrustedHosts。

> [!NOTE]
> 處於工作群組環境，或在網域中使用本機系統管理員認證時，需要修改 TrustedHosts。 如果您選擇省略這項設定，就必須[手動設定 TrustedHosts](../support/troubleshooting.md#configure-trustedhosts)。

當您從 **\[開始\]** 功能表啟動 Windows Admin Center 時，它會在預設瀏覽器中開啟。

第一次啟動 Windows Admin Center 時，您會在桌面的通知區域中看到圖示。 在此圖示上按一下滑鼠右鍵，然後選擇 **\[開啟\]** 在預設瀏覽器中開啟工具，或選擇 **\[結束\]** 以結束背景處理程序。

## <a name="install-on-windows-server-with-desktop-experience"></a>安裝在具有桌面體驗的 Windows Server

在 Windows Server 上，Windows Admin Center 會安裝為網路服務。 您必須指定服務接聽的連接埠，而且此服務需要 HTTPS 的憑證。 安裝程式可以建立自我簽署憑證以供測試，您也可以提供電腦上已安裝憑證的指紋。 如果您使用產生的憑證，此憑證將會與伺服器的 DNS 名稱相符。 如果您使用您自己的憑證，請確定憑證中所提供的名稱符合電腦名稱 （萬用字元憑證不支援）。您也可以選擇要讓管理您的 TrustedHosts 的 Windows Admin Center。

> [!NOTE]
> 處於工作群組環境，或在網域中使用本機系統管理員認證時，需要修改 TrustedHosts。 如果您選擇省略這項設定，您必須[手動設定 TrustedHosts](../support/troubleshooting.md#configure-trustedhosts)。

安裝完成之後，從遠端電腦開啟瀏覽器並瀏覽至安裝程式的最後一個步驟所述的 URL。

> [!WARNING]
> 自動產生的憑證將於安裝後 60 天過期。

## <a name="install-on-server-core"></a>安裝在 Server Core

如果您有 Windows Server 的 Server Core 安裝，則可以從命令提示字元 (以系統管理員身分執行) 安裝 Windows Admin Center。 分別使用 `SME_PORT` 和 `SSL_CERTIFICATE_OPTION` 引數，指定連接埠和 SSL 憑證。 如果您要使用現有的憑證，請使用 `SME_THUMBPRINT` 來指定其憑證指紋。

> [!WARNING]
> 安裝 Windows Admin Center 將會重新啟動 WinRM 服務，這將會斷絕所有遠端的 PowerShells 工作階段。 建議您從本機的 Cmd 或 PowerShell 安裝。 如果您要安裝會由 WinRM 服務重新啟動中斷的自動化解決方案，您可以將參數加入```RESTART_WINRM=0```安裝引數，但 WinRM 必須重新啟動 Windows Admin Center 函式。

執行下列命令以安裝 Windows Admin Center，並自動產生自我簽署憑證。

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

執行下列命令以使用現有憑證來安裝 Windows Admin Center：

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> 不要使用點斜線相對路徑標記法 (例如，`.\<WindowsAdminCenterInstallerName>.msi`)，從 PowerShell 叫用 `msiexec`。 不支援將該標記法，安裝將會失敗。 移除 `.\` 前置詞，或指定 MSI 的完整路徑。

## <a name="updating-windows-admin-center"></a>更新的 Windows Admin Center

使用 Microsoft Update，或以手動方式安裝，您可以更新 Windows Admin Center 非預覽的版本。 

升級至新版的 Windows Admin Center 時，會保留您的設定。 我們不正式支援升級 Insider Preview 版本的 Windows Admin Center-我們認為是比較理想的全新安裝-，但我們不會封鎖它。