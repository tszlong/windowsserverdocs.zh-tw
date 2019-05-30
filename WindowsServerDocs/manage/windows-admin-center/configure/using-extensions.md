---
title: 安裝和管理擴充功能
description: 安裝和管理 Windows Admin Center （專案檀香山） 中的延伸模組
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c775dd5a3011115bbb031c0b9e4e24a8911d378e
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63748401"
---
# <a name="install-and-manage-extensions"></a>安裝和管理擴充功能

>適用於：Windows Admin Center，Windows Admin Center 預覽

Windows Admin Center 是建置為可延伸的平台，每個連接類型和工具時，您可以安裝、 解除安裝和更新個別擴充功能。 您可以搜尋由 Microsoft 與其他開發人員，發行新的擴充功能和安裝個別更新而不需要更新整個 Windows Admin Center 安裝。 您也可以設定個別的 NuGet 摘要或檔案共用，並將在內部使用，您組織中的擴充功能散發。

## <a name="installing-an-extension"></a>安裝擴充功能

Windows Admin Center 會顯示可用的擴充功能，從指定的 NuGet 摘要。 根據預設，Windows Admin Center 會指向 Microsoft 官方 NuGet 摘要裝載由 Microsoft 與其他開發人員發行的擴充功能。

1. 按一下 **設定**在右上角的按鈕 > 在左窗格中，按一下**延伸模組**。 
2. **可用的擴充功能** 索引標籤會列出摘要之延伸模組可供安裝。
3. 按一下 擴充功能來檢視擴充功能描述、 版本、 發行者和中的其他資訊**詳細資料**窗格。
4. 按一下 **安裝**安裝延伸模組。 如果閘道必須執行在提升權限模式下，才能進行這項變更，將會看到 UAC 提高權限提示。 安裝完成後，會自動重新整理您的瀏覽器和 Windows Admin Center 將會重新載入新安裝的擴充功能。 如果您嘗試擴充功能安裝是先前安裝的擴充功能的更新，您可以按一下**更新為最新**安裝更新 按鈕。 您也可以移至**已安裝擴充功能**tab 鍵移至檢視已安裝擴充功能，並查看是否有更新中提供**狀態**資料行。

## <a name="installing-extensions-from-a-different-feed"></a>安裝延伸模組，從不同的摘要

Windows Admin Center 支援多個摘要，而且您可以檢視和管理封裝從一個以上的摘要，一次。 任何 NuGet 摘要的支援 NuGet V2 Api 或檔案共用可以安裝擴充功能新增至 Windows Admin Center。

1. 按一下 **設定**在右上角的按鈕 > 在左窗格中，按一下**延伸模組**。
2. 在右窗格中，按一下**摘要** 索引標籤。
3. 按一下 **新增**按鈕以新增另一個的摘要。 NuGet 摘要，請輸入摘要 URL NuGet V2。 NuGet 摘要提供者或系統管理員應該能夠提供的 URL 資訊。 檔案共用，請輸入檔案的完整路徑 (.nupkg) 的延伸模組套件檔案會儲存共用的。
4. 按一下 **\[新增\]** 。 如果閘道必須執行在提升權限模式下，才能進行這項變更，將會看到 UAC 提高權限提示。

**可用的擴充功能**清單會顯示從所有已註冊的延伸模組。 您可以檢查每個延伸模組是使用哪一個摘要**套件摘要**資料行。

## <a name="uninstalling-an-extension"></a>解除安裝擴充功能

您可以解除安裝任何擴充功能，您先前已安裝，或甚至是解除安裝已預先安裝的 Windows Admin Center 安裝一部分的任何工具。

1. 按一下 **設定**在右上角的按鈕 > 在左窗格中，按一下**延伸模組**。 
2. 按一下 **已安裝擴充功能**索引標籤來檢視所有已安裝的延伸模組。
3. 選擇以解除安裝，然後按一下 擴充功能**解除安裝**。

在解除安裝後之後，會自動重新整理您的瀏覽器，然後將重新載入移除擴充功能的 Windows Admin Center。 如果您解除安裝已預先安裝的 Windows Admin Center 一部分的工具，此工具將可供在重新安裝**可用的擴充功能** 索引標籤。

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>沒有網際網路連線的電腦上安裝擴充功能

如果在未連線到網際網路或位於 proxy 後方的電腦上安裝 Windows Admin Center，則它可能無法存取，並從摘要的 Windows Admin Center 安裝的擴充功能。 您可以下載延伸模組套件，以手動方式或透過 PowerShell 指令碼，並設定要擷取封裝從檔案共用或本機磁碟機的 Windows Admin Center。

### <a name="manually-downloading-extension-packages"></a>手動下載延伸模組套件

1. 在具有網際網路連線的另一部電腦，開啟網頁瀏覽器並瀏覽至下列 URL: [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

  * 您可能需要建立帳戶，msft sme.myget.org 和登入若要檢視延伸模組套件。

2. 按一下您想要檢視套件詳細資料頁面安裝封裝的名稱。
3. 按一下 **下載**連結套件詳細資料頁面的右側窗格中，並下載.nupkg 檔案延伸模組。
4. 針對您想要下載的所有封裝重複步驟 2 和 3。
5. 可以從 Windows Admin Center，安裝的電腦存取檔案共用或本機電腦的磁碟，請將複製的封裝檔案。
6. [請依照下列指示來安裝延伸模組，從不同的摘要](#installing-extensions-from-a-different-feed)。

### <a name="downloading-packages-with-a-powershell-script"></a>下載 PowerShell 指令碼的封裝

下載 NuGet 套件的 NuGet 摘要從網際網路上有許多指令碼可用。 我們將使用[Jon Galloway 所提供的指令碼](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)，Microsoft 的資深專案經理。

1. 中所述[部落格文章](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)、 將指令碼安裝為 NuGet 套件，或複製並貼入 PowerShell ISE 中的指令碼。
2. 編輯第一行的指令碼，您的 NuGet 摘要的 v2 URL。 如果您要下載 Windows Admin Center 官方的套件摘要，請使用下列 URL。

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. 執行指令碼，它會從摘要下載所有的 NuGet 套件，到下列本機資料夾： %USERPROFILE%\Documents\NuGetLocal
4. [請依照下列指示來安裝延伸模組，從不同的摘要](#installing-extensions-from-a-different-feed)。

## <a name="manage-extensions-with-powershell"></a>管理使用 PowerShell 的擴充功能

>適用於：Windows Admin Center，Windows Admin Center 預覽

Windows Admin Center Preview 包含 PowerShell 模組來管理您的閘道擴充功能。

```powershell
# Add the module to the current session
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ExtensionTools"
# Available cmdlets: Get-Feed, Add-Feed, Remove-Feed, Get-Extension, Install-Extension, Uninstall-Extension, Update-Extension

# List feeds
Get-Feed "https://wac.contoso.com"

# Add a new extension feed
Add-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# Remove an extension feed
Remove-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# List all extensions
Get-Extension "https://wac.contoso.com"

# Install an extension (locate the latest version from all feeds and install it)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers"

# Install an extension (latest version from a specific feed, if the feed is not present, it will be added)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers" -Feed "https://aka.ms/sme-extension-feed"

# Install an extension (install a specific version)
Install-Extension "https://wac.contoso.com" "msft.sme.certificate-manager" "0.133.0"

# Uninstall-Extension
Uninstall-Extension "https://wac.contoso.com" "msft.sme.containers"

# Update-Extension
Update-Extension "https://wac.contoso.com" "msft.sme.containers"
```

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[了解如何建置 Windows Admin Center SDK 延伸模組](../extend/extensibility-overview.md)。