---
title: 開發工具擴充功能
description: '開發工具延伸模組 Windows Admin Center SDK (專案 Honolulu) '
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9cf369558977c50ee0913b65e62de448959aac32
ms.sourcegitcommit: f89639d3861c61620275c69f31f4b02fd48327ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/29/2020
ms.locfileid: "91517574"
---
# <a name="develop-a-tool-extension"></a>開發工具擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

工具延伸是使用者與 Windows Admin Center 互動以管理連線（例如伺服器或叢集）的主要方式。 當您在 Windows Admin Center 首頁] 畫面上按一下連接並聯機時，您會在左側流覽窗格中看到工具清單。 當您按一下工具時，工具擴充功能就會載入，並顯示在右窗格中。

載入工具延伸模組時，它可以在目標伺服器或叢集上執行 WMI 呼叫或 PowerShell 腳本，並在 UI 中顯示資訊，或根據使用者輸入來執行命令。 工具延伸模組會定義應顯示的解決方案，並為每個解決方案產生一組不同的工具。

> [!NOTE]
> 不熟悉不同的延伸模組類型嗎？ 深入瞭解延伸模組 [架構和延伸模組類型](understand-extensions.md)。

## <a name="prepare-your-environment"></a>準備您的環境

如果您還沒有這麼做，請安裝所有專案所需的相依性和全域必要條件，以 [準備您的環境](prepare-development-environment.md) 。

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>使用 Windows Admin Center CLI 建立新的工具延伸模組 ##

安裝所有相依性之後，您就可以開始建立新的工具延伸模組。  建立或流覽至包含您專案檔的資料夾，開啟命令提示字元，並將該資料夾設定為工作目錄。  使用先前安裝的 Windows Admin Center CLI，以下列語法建立新的擴充功能：

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱 (空格)  | ```Contoso Inc``` |
| ```{!Tool Name}``` | 您的工具名稱 (空格)  | ```Manage Foo Works``` |

這裡提供一個範例用法：

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

這會使用您為工具指定的名稱，在目前的工作目錄內建立新的資料夾，並將所有必要的範本檔案複製到您的專案中，並使用您的公司和工具名稱設定這些檔案。

接下來，將目錄變更至剛才建立的資料夾，然後執行下列命令來安裝必要的本機相依性：

``` cmd
npm install
```

完成之後，您就可以設定將新擴充功能載入 Windows Admin Center 所需的一切。

## <a name="add-content-to-your-extension"></a>將內容新增至您的延伸模組

現在您已使用 Windows Admin Center CLI 建立擴充功能，接下來您可以自訂內容。  如需您可以執行之動作的範例，請參閱下列指南：

- 新增 [空白模組](guides/add-module.md)
- 新增 [iFrame](guides/add-iframe.md)

您可以在我們的開發人員指南中找到更多範例。 開發人員指南是功能完整的解決方案延伸模組，可以側載到 Windows Admin Center 中，並包含一組豐富的範例功能和工具範例，可讓您在自己的延伸模組中流覽和使用。 

在 Windows Admin Center 設定的 [ **Advanced** ] 頁面上啟用開發人員指南延伸模組。 

## <a name="customize-your-extensions-icon"></a>自訂您的延伸模組圖示

您可以自訂在工具清單中為您的延伸模組顯示的圖示。  若要這樣做，請 ```icon``` ```manifest.json``` 針對您的延伸模組修改中的所有專案：

``` json
"icon": "{!icon-uri}",
```

| 值 | 說明 | 範例 uri |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | 圖示資源的位置 | ```assets/foo-icon.svg``` |

注意：目前，在開發模式中側載您的延伸模組時，不會顯示自訂圖示。  若要解決此問題，請移除的內容 ```target``` 如下：

``` json
"target": "",
```

這項設定僅適用于在開發模式下端載入，因此請務必保留中包含的值， ```target``` 然後在發佈您的延伸模組之前將其還原。

## <a name="build-and-side-load-your-extension"></a>組建和並存載入您的延伸模組

接下來，建立您的延伸模組，並將其並存載入 Windows Admin Center 中。  開啟命令視窗，將目錄變更為您的來原始目錄，然後您就可以開始建立。

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

使用 SDK 變更和平臺變更讓您的延伸模組保持最新狀態很簡單。  瞭解如何以 [不同版本](target-sdk-version.md) 的 Windows Admin Center SDK 為目標。