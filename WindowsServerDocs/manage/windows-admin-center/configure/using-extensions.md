---
title: 安裝和管理擴充功能
description: 在 Windows Admin Center 中安裝和管理擴充功能 (Honolulu 專案)
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.openlocfilehash: c2feaaff614d00afeaf5d132c446eebe5fdf0989
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966775"
---
# <a name="install-and-manage-extensions"></a>安裝和管理擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows Admin Center 是建置在可擴充的平台上，其中每個連線類型和工具都是您可以個別安裝、解除安裝和更新的擴充功能。 您可以搜尋 Microsoft 和其他開發人員所發行的新擴充功能，並個別安裝和更新，而不需要更新整個 Windows Admin Center 的安裝。 您也可以設定個別的 NuGet 摘要或檔案共用，並將擴充功能散發給組織內部使用。

## <a name="installing-an-extension"></a>安裝擴充功能

Windows Admin Center 會顯示指定 NuGet 摘要中可用的擴充功能。 根據預設，Windows Admin Center 會指向 Microsoft 官方 NuGet 摘要，其裝載由 Microsoft 和其他開發人員所發行的擴充功能組。

1. 按一下右上方的 [設定] 按鈕，然後在左側窗格中按一下 [擴充功能]。
2. [可用擴充功能] 索引標籤會列出摘要上可供安裝的擴充功能。
3. 按一下擴充功能，即可在 [詳細資料] 窗格中檢視擴充功能描述、版本、發行者和其他資訊。
4. 按一下 [安裝] 來安裝擴充功能。 如果閘道必須在提高權限的模式中執行才能進行此變更，您會看到 UAC 提高權限的提示。 安裝完成之後，您的瀏覽器將會自動重新整理，而且 Windows Admin Center 將會重新載入並安裝新的擴充功能。 如果您嘗試安裝的擴充功能是先前所安裝擴充功能的更新，您可以按一下 [更新為最新版本] 按鈕來安裝更新。 您也可以移至 [安裝的擴充功能] 索引標籤來檢視已安裝的擴充功能，並查看 [狀態] 資料行中是否有可用的更新。

## <a name="installing-extensions-from-a-different-feed"></a>從不同的摘要安裝擴充功能

Windows Admin Center 支援多個摘要，而且您可以一次從多個摘要中檢視和管理套件。 任何支援 NuGet V2 API 或檔案共用的 NuGet 摘要，都可以新增至 Windows Admin Center，以便從中安裝擴充功能。

1. 按一下右上方的 [設定] 按鈕，然後在左側窗格中按一下 [擴充功能]。
2. 在右側窗格中，按一下 [摘要] 索引標籤。
3. 按一下 [新增] 按鈕，即可新增另一個摘要。 針對 NuGet 摘要，輸入 NuGet V2 摘要 URL。 NuGet 摘要提供者或系統管理員應該能夠提供 URL 資訊。 針對檔案共用，請輸入儲存擴充套件檔案 (.nupkg) 的檔案共用完整路徑。
4. 按一下 **[新增]** 。 如果閘道必須在提高權限的模式中執行才能進行此變更，您會看到 UAC 提高權限的提示。 只有當您以桌面模式執行 Windows Admin Center 時，才會出現此提示。

**可用擴充功能**清單會顯示所有已註冊摘要中的擴充功能。 您可以使用**套件摘要**資料行，檢查每個擴充功能來自哪一個摘要。

## <a name="uninstalling-an-extension"></a>解除安裝擴充功能

您可以解除安裝任何先前已安裝的擴充功能，或甚至解除安裝已預先安裝為 Windows Admin Center 安裝一部分的任何工具。

1. 按一下右上方的 [設定] 按鈕，然後在左側窗格中按一下 [擴充功能]。
2. 按一下 [安裝的擴充功能] 索引標籤來檢視所有已安裝的擴充功能。
3. 選擇要解除安裝的擴充功能，然後按一下 [解除安裝]。

解除安裝完成之後，您的瀏覽器將會自動重新整理，而且 Windows Admin Center 將會重新載入並將擴充功能移除。 如果您要解除安裝的工具已預先安裝為 Windows Admin Center 的一部分，則可以在 [可用擴充功能] 索引標籤中重新安裝此工具。

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>在沒有網際網路連線的電腦上安裝擴充功能

如果 Windows Admin Center 安裝在未連線到網際網路或位於 Proxy 後方的電腦上，則可能無法從 Windows Admin Center 摘要存取並安裝擴充功能。 您可以透過手動方式或使用 PowerShell 指令碼下載擴充套件，並將 Windows Admin Center 設定為從檔案共用或本機磁碟機擷取套件。

### <a name="manually-downloading-extension-packages"></a>手動下載擴充套件

1. 在另一部具有網際網路連線的電腦上，開啟網頁瀏覽器並瀏覽至下列 URL：[https://dev.azure.com/WindowsAdminCenter/Windows%20Admin%20Center%20Feed/_packaging?_a=feed&feed=WAC](https://dev.azure.com/WindowsAdminCenter/Windows%20Admin%20Center%20Feed/_packaging?_a=feed&feed=WAC)

   * 您可能需要建立 Microsoft 帳戶，並登入以查看擴充套件。

2. 按一下您要安裝的套件名稱，以檢視套件詳細資料頁面。
3. 在 [套件詳細資料] 頁面的頂端瀏覽列中按一下 [下載] 連結，並下載擴充功能的 .nupkg 檔案。
4. 針對您想要下載的所有套件，重複步驟 2 和 3。
5. 將套件檔案複製到檔案共用 (可從安裝 Windows Admin Center 的電腦進行存取)，或複製到電腦的本機磁碟。
6. [請遵循指示，從不同的摘要中安裝擴充功能](#installing-extensions-from-a-different-feed)。

### <a name="downloading-packages-with-a-powershell-script"></a>使用 PowerShell 指令碼下載套件

網際網路上有許多可用來從 NuGet 摘要下載 NuGet 套件的指令碼。 我們將使用 Microsoft 資深專案經理 [Jon Galloway 所提供的指令碼](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)。

1. 如[部落格文章](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell)所述，將指令碼安裝為 NuGet 套件，或將指令碼複製並貼到 PowerShell ISE。
2. 將指令碼的第一行編輯為 NuGet 摘要的 v2 URL。 如果您要從 Windows Admin Center 官方摘要中下載套件，請使用下列 URL。

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. 執行指令碼，即可將摘要中的所有 NuGet 套件下載到下列本機資料夾：%USERPROFILE%\Documents\NuGetLocal
4. [請遵循指示，從不同的摘要中安裝擴充功能](#installing-extensions-from-a-different-feed)。

## <a name="manage-extensions-with-powershell"></a>使用 PowerShell 管理擴充功能

Windows Admin Center 預覽版包含可管理您閘道擴充功能的 PowerShell 模組。

[!INCLUDE [ps-extensions](../includes/ps-extensions.md)]

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdk"></a>[深入了解如何使用 Windows Admin Center SDK 建置擴充功能](../extend/extensibility-overview.md)。
