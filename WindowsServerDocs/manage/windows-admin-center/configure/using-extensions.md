---
title: 安裝和管理擴充功能
description: 在 Windows 系統管理中心安裝和管理擴充功能（Project 檀香山）
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d49e25591c705afa217b2332ee48eb42c5c2f7ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357237"
---
# <a name="install-and-manage-extensions"></a>安裝和管理擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows 管理中心是以可擴充的平臺建立，其中每個連線類型和工具都是您可以個別安裝、卸載和更新的擴充功能。 您可以搜尋 Microsoft 和其他開發人員所發行的新延伸模組，並個別安裝和更新，而不需要更新整個 Windows 系統管理中心安裝。 您也可以設定個別的 NuGet 摘要或檔案共用，並將延伸模組散發給組織內部使用。

## <a name="installing-an-extension"></a>安裝擴充功能

Windows 管理中心會顯示指定之 NuGet 摘要提供的延伸模組。 根據預設，Windows 管理中心會指向 Microsoft 官方 NuGet 摘要，其裝載由 Microsoft 和其他開發人員所發行的延伸模組。

1. 在左窗格中，按一下右上方 > 的 [**設定**] 按鈕，然後按一下 [**擴充**功能]。 
2. [**可用的延伸**模組] 索引標籤會列出可供安裝的摘要上的延伸模組。
3. 按一下延伸模組，即可在**詳細資料**窗格中查看延伸模組描述、版本、發行者和其他資訊。
4. 按一下 [**安裝**] 以安裝擴充功能。 如果閘道必須在提高許可權的模式下執行才能進行這項變更，您會看到 UAC 提高權限提示。 安裝完成之後，您的瀏覽器將會自動重新整理，而且 Windows 管理中心將會重載並安裝新的延伸模組。 如果您嘗試安裝的延伸模組是先前安裝之延伸模組的更新，您可以按一下 [**更新為最新**的] 按鈕來安裝更新。 您也可以移至 [**已安裝的擴充**功能] 索引標籤來查看已安裝的擴充功能，並查看 [**狀態**] 欄是否有可用的更新。

## <a name="installing-extensions-from-a-different-feed"></a>從不同的摘要安裝延伸模組

Windows 系統管理中心支援多個摘要，而且您可以一次從多個摘要中查看和管理套件。 任何支援 NuGet V2 Api 或檔案共用的 NuGet 摘要，都可以新增至 Windows 系統管理中心，以便從安裝延伸模組。

1. 在左窗格中，按一下右上方 > 的 [**設定**] 按鈕，然後按一下 [**擴充**功能]。
2. 在右窗格中，按一下 [**摘要] 索引**標籤。
3. 按一下 [**新增**] 按鈕以新增另一個摘要。 針對 NuGet 摘要，輸入 NuGet V2 摘要 URL。 NuGet 摘要提供者或系統管理員應該能夠提供 URL 資訊。 針對檔案共用，輸入儲存延伸模組套件檔案（. nupkg）之檔案共用的完整路徑。
4. 按一下 [新增]。 如果閘道必須在提高許可權的模式下執行才能進行這項變更，您會看到 UAC 提高權限提示。

[**可用的延伸**模組] 清單會顯示所有已註冊摘要的延伸模組。 您可以使用 [**封裝**摘要] 資料行來檢查每個延伸模組的摘要。

## <a name="uninstalling-an-extension"></a>卸載擴充功能

您可以卸載任何先前已安裝的擴充功能，或甚至卸載已預先安裝為 Windows 管理中心安裝一部分的任何工具。

1. 在左窗格中，按一下右上方 > 的 [**設定**] 按鈕，然後按一下 [**擴充**功能]。 
2. 按一下 [**已安裝的延伸**模組] 索引標籤，以查看所有已安裝的擴充
3. 選擇要卸載的擴充功能，然後按一下 [**卸載**]。

卸載完成之後，您的瀏覽器將會自動重新整理，而且 Windows 管理中心將會重載並移除延伸模組。 如果您卸載的工具已預先安裝為 Windows 系統管理中心的一部分，則可在 [**可用的擴充**功能] 索引標籤中重新安裝此工具。

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>在沒有網際網路連線的電腦上安裝擴充功能

如果 Windows 管理中心安裝在未連線到網際網路或位於 proxy 後方的電腦上，則可能無法從 Windows 系統管理中心摘要存取並安裝擴充功能。 您可以手動或使用 PowerShell 腳本下載延伸模組套件，並將 Windows 管理中心設定為從檔案共用或本機磁片磁碟機取出封裝。

### <a name="manually-downloading-extension-packages"></a>手動下載延伸模組套件

1. 在另一部具有網際網路連線能力的電腦上，開啟網頁瀏覽器並流覽至下列 URL： [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

   * 您可能需要在 msft-sme.myget.org 上建立帳戶，並登入以查看延伸模組套件。

2. 按一下您要安裝之套件的名稱，以查看套件詳細資料頁面。
3. 在 [套件詳細資料] 頁面的右窗格中，按一下 [**下載**] 連結，並下載延伸模組的 nupkg 檔案。
4. 針對您想要下載的所有套件，重複步驟2和3。
5. 將套件檔案複製到檔案共用，以從 Windows 系統管理中心安裝的電腦或電腦的本機磁片上存取。
6. [請遵循指示，從不同的摘要安裝延伸](#installing-extensions-from-a-different-feed)模組。

### <a name="downloading-packages-with-a-powershell-script"></a>使用 PowerShell 腳本下載封裝

網際網路上有許多可用來從 NuGet 摘要下載 NuGet 套件的腳本。 我們將使用 Microsoft 的資深專案經理[Jon Galloway 所提供的腳本](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)。

1. 如[blog 文章](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)中所述，將腳本安裝為 NuGet 套件，或將腳本複製並貼到 PowerShell ISE。
2. 將腳本的第一行編輯為 NuGet 摘要的 v2 URL。 如果您要從 Windows 管理中心官方摘要下載封裝，請使用下列 URL。

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. 執行腳本，它會將摘要中的所有 NuGet 套件下載到下列本機資料夾：%USERPROFILE%\Documents\NuGetLocal
4. [請遵循指示，從不同的摘要安裝延伸](#installing-extensions-from-a-different-feed)模組。

## <a name="manage-extensions-with-powershell"></a>使用 PowerShell 管理延伸模組

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows 系統管理中心預覽包含 PowerShell 模組來管理您的閘道擴充功能。

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

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[深入瞭解如何使用 Windows Admin CENTER SDK 建立擴充](../extend/extensibility-overview.md)功能。