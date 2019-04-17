---
title: 安裝和管理延伸模組
description: 安裝和管理 Windows Admin Center (Project Honolulu) 中的延伸模組
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c775dd5a3011115bbb031c0b9e4e24a8911d378e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296700"
---
# 安裝和管理延伸模組

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows Admin Center 建置為可延伸的平台，其中每個連線類型和工具是您可以安裝、 解除安裝和更新個別的擴充功能。 您可以搜尋 Microsoft 和其他開發人員所發佈的新擴充功能和安裝與個別更新而不需要更新整個 Windows Admin Center 安裝。 您也可以設定個別的 NuGet 摘要或檔案共用，並散布您的組織內內部使用的擴充功能。

## 安裝擴充功能

Windows Admin Center 將會顯示可用的擴充功能，從指定的 NuGet 摘要。 根據預設，Windows Admin Center 指向摘要 Microsoft 官方 NuGet 主控 Microsoft 和其他開發人員所發佈的擴充功能。

1. 按一下右上方 >，在左窗格中的 [**設定**] 按鈕，按一下 [**擴充功能**。 
2. [**可用的擴充功能**] 索引標籤，將會列出可用於安裝摘要擴充功能。
3. 按一下 [檢視**詳細資料**窗格中的擴充功能描述、 版本、 發行者及其他資訊的擴充功能。
4. 按一下 [**安裝**]，以安裝擴充功能。 如果閘道必須進行這項變更的提升權限模式中執行，您將會顯示 UAC 提高權限提示。 安裝完成後，將會自動重新整理您的瀏覽器和 Windows Admin Center 將會重新載入新安裝的副檔名。 如果您嘗試安裝延伸模組是先前已安裝的擴充功能的更新，您可以按一下 [**更新至最新版本**的按鈕，以安裝更新。 您也可以移至 [**已安裝的擴充功能**] 索引標籤檢視已安裝的擴充功能，並查看是否有**狀態**] 欄中可用的更新。

## 從不同的摘要安裝擴充功能

Windows Admin Center 支援多個同步發佈摘要，而您可以檢視和管理套件從一個以上的摘要，一次。 任何 NuGet 摘要該支援 NuGet V2 Api 或檔案共用可以新增至 Windows Admin Center 的安裝從擴充功能。

1. 按一下右上方 >，在左窗格中的 [**設定**] 按鈕，按一下 [**擴充功能**。
2. 在右窗格中，按一下 [**摘要**] 索引標籤。
3. 按一下 [**加入**] 按鈕以新增另一個摘要。 NuGet 摘要，請輸入 NuGet V2 摘要的 URL。 NuGet 摘要提供者或系統管理員應該能夠提供 URL 的資訊。 檔案共用，請輸入之檔案的完整路徑中的延伸模組套件檔案 (.nupkg) 會儲存共用。
4. 按一下 **\[新增\]**。 如果閘道必須進行這項變更的提升權限模式中執行，您將會顯示 UAC 提高權限提示。

**可用的擴充功能**清單會顯示從所有已註冊的同步發佈摘要的擴充功能。 您可以檢查每個延伸模組是從使用**套件摘要**資料行的摘要。

## 解除安裝擴充功能

您可以解除安裝您先前已安裝，任何擴充功能，或甚至解除安裝任何已預先安裝 Windows Admin Center 安裝程序的工具。

1. 按一下右上方 >，在左窗格中的 [**設定**] 按鈕，按一下 [**擴充功能**。 
2. 按一下 [**安裝擴充功能**] 索引標籤，若要檢視所有已安裝的擴充功能。
3. 選擇 [擴充功能來解除安裝，然後按一下 [**解除安裝**。

解除安裝後是完成、 自動重新整理您的瀏覽器和 Windows Admin Center 將會重新載入與移除延伸模組。 如果您解除安裝已預先安裝 Windows Admin Center 的一部分的工具，此工具會提供**可用的擴充功能**] 索引標籤中重新安裝。

## 在沒有網際網路連線的電腦上安裝擴充功能

如果無法連線到網際網路或位於 proxy 後方的電腦上安裝 Windows Admin Center，則它可能無法存取，並從摘要 Windows Admin Center 安裝延伸模組。 您可以下載延伸模組套件，以手動方式或使用 PowerShell 指令碼，並設定 Windows Admin Center 來擷取套件從檔案共用或本機磁碟機。

### 手動下載延伸模組套件

1. 有網際網路連線的另一部電腦，開啟網頁瀏覽器並瀏覽到下列 URL:[https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

  * 您可能需要在 msft sme.myget.org 和登入，若要檢視延伸模組套件建立帳戶。

2. 按一下您想要檢視套件詳細資料頁面安裝套件的名稱。
3. 按一下右側窗格的 [套件詳細資料頁面中的 [**下載**] 連結，然後下載.nupkg 檔案的擴充功能。
4. 適用於您想要下載的所有套件，請重複步驟 2 和 3。
5. 從電腦，安裝 Windows Admin Center 可存取的檔案共用或本機磁碟的電腦，請複製套件檔案。
6. [請依照下列指示安裝擴充功能，從不同的摘要](#installing-extensions-from-a-different-feed)。

### 下載 PowerShell 指令碼與套件

下載 NuGet 套件從摘要 NuGet 網際網路上有許多的指令碼可用。 我們會使用[Jon Galloway 所提供的指令碼](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)，在 Microsoft 資深程式管理員。

1. 中所述的[部落格文章](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)，安裝指令碼做為 NuGet 套件或複製和貼上至 PowerShell ISE 的指令碼。
2. 編輯您 NuGet 指令碼的第一行摘要的 v2 URL。 如果您下載套件從正式 Windows Admin Center 摘要，請使用下列的 URL。

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. 執行指令碼，它將會下載所有的 NuGet 套件從摘要下列的本機資料夾： %USERPROFILE%\Documents\NuGetLocal
4. [請依照下列指示安裝擴充功能，從不同的摘要](#installing-extensions-from-a-different-feed)。

## 管理延伸模組，使用 PowerShell

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows Admin Center 預覽版包含 PowerShell 模組以管理您的閘道延伸模組。

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

### [深入了解有關建置使用 Windows Admin Center SDK 擴充功能](../extend/extensibility-overview.md)。