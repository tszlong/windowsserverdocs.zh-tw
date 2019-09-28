---
title: 開發工具擴充功能
description: 開發工具擴充功能 Windows 系統管理中心 SDK （Project 檀香山）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: de2cbf3a47771555eef02cd7d18f93b2b33227b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406906"
---
# <a name="develop-a-tool-extension"></a>開發工具擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

工具擴充功能是使用者與 Windows 管理中心進行互動以管理連接的主要方式，例如伺服器或叢集。 當您按一下 [Windows 管理中心] 主畫面中的連線並連接時，您會在左側流覽窗格中看到一份工具清單。 當您按一下工具時，會載入工具擴充功能，並在右窗格中顯示。

載入工具擴充功能時，它可以在目標伺服器或叢集上執行 WMI 呼叫或 PowerShell 腳本，並在 UI 中顯示資訊，或根據使用者輸入來執行命令。 工具擴充會定義應該顯示的解決方案，為每個解決方案產生一組不同的工具。

> [!NOTE]
> 不熟悉不同的延伸模組類型嗎？ 深入瞭解延伸模組[架構和擴充功能類型](understand-extensions.md)。

## <a name="prepare-your-environment"></a>準備您的環境

如果您還沒有這麼做，請安裝所有專案所需的相依性和全域必要條件來[準備您的環境](prepare-development-environment.md)。

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>使用 Windows Admin Center CLI 建立新的工具擴充功能 ##

安裝所有相依性之後，您就可以開始建立新的工具擴充功能。  建立或流覽至包含您專案檔的資料夾，開啟命令提示字元，並將該資料夾設定為工作目錄。  使用先前安裝的 Windows Admin Center CLI，以下列語法建立新的擴充功能：

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱（含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 您的工具名稱（含空格） | ```Manage Foo Works``` |

這裡提供一個範例用法：

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

這會使用您為工具指定的名稱，在目前的工作目錄內建立新的資料夾，將所有必要的範本檔案複製到您的專案中，並使用您的公司和工具名稱來設定檔案。  

接下來，將目錄切換到剛才建立的資料夾，然後執行下列命令來安裝必要的本機相依性：

``` cmd
npm install
```

完成後，您就可以設定將新延伸模組載入 Windows 管理中心所需的所有專案。 

## <a name="add-content-to-your-extension"></a>將內容新增至您的擴充功能

既然您已使用 Windows 管理中心 CLI 建立擴充功能，就可以開始自訂內容。  如需您可以執行之動作的範例，請參閱下列指南：

- 新增[空白模組](guides/add-module.md)
- 新增[iFrame](guides/add-iframe.md)
 
如需更多範例，請參閱我們的[GITHUB SDK 網站](https://aka.ms/wacsdk)：
-  [開發人員工具](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools)是功能完整的延伸模組，可以側載到 Windows 系統管理中心，而且包含豐富的範例功能和工具範例集合，可讓您在自己的延伸模組中流覽和使用。

## <a name="customize-your-extensions-icon"></a>自訂您的擴充功能圖示

您可以在 [工具] 清單中自訂您的擴充功能所顯示的圖示。  若要這麼做，請針對您的延伸模組修改 ```manifest.json``` 中的所有 ```icon``` 專案：

``` json
"icon": "{!icon-uri}",
```

| 值 | 說明 | 範例 uri |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | 圖示資源的位置 | ```assets/foo-icon.svg``` |

注意：目前，當您在 dev 模式中載入擴充功能時，不會顯示自訂圖示。  因應措施是移除 ```target``` 的內容，如下所示：

``` json
"target": "",
```

此設定僅適用于在 dev 模式中進行側載，因此請務必保留包含在 ```target``` 的值，然後在發佈擴充功能之前將它還原。

## <a name="build-and-side-load-your-extension"></a>組建和側邊載入您的擴充功能

接下來，建立您的延伸模組並將其載入至 Windows 系統管理中心。  開啟命令視窗，將目錄變更為您的來原始目錄，然後您就可以開始建立。

* 建置，並搭配 gulp 使用：

    ``` cmd
    gulp build
    gulp serve -p 4201
    ```

請注意，您必須選擇目前未使用的連接埠。 請確定您嘗試使用的不是 Windows Admin Center 正在用來執行的連接埠。

您可在 Windows Admin Center 中附加由本機服務的專案，以將專案側載至 Windows Admin Center 的本機執行個體供測試。

* 在網頁瀏覽器中啟動 Windows Admin Center
* 開啟偵錯工具 (F12)
* 開啟主控台，並鍵入下列命令：

    ``` cmd
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   重新整理網頁瀏覽器

您的專案現在會顯示在 [工具] 清單中，名稱旁邊有 (已側載) 標示。

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>以不同版本的 Windows Admin Center SDK 為目標

使用 SDK 變更和平臺變更讓擴充功能保持最新狀態很容易。  瞭解如何以[不同版本](target-sdk-version.md)的 WINDOWS 管理中心 SDK 為目標。