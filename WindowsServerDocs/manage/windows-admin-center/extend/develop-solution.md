---
title: 開發解決方案擴充功能
description: 開發解決方案延伸模組 Windows 系統管理中心 SDK （Project 檀香山）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ac9c6296fdf9159c9f50a1304dd345932052ac9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357152"
---
# <a name="develop-a-solution-extension"></a>開發解決方案擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

解決方案主要會定義您想要透過 Windows 管理中心管理的唯一物件類型。  這些解決方案/連線類型預設包含在 Windows 管理中心內：

* Windows Server 連接
* Windows 電腦連線
* 容錯移轉叢集連接
* 超交集叢集連接

當您從 [Windows 管理中心連線] 頁面選取連接時，會載入該連線類型的解決方案延伸模組，而 Windows 管理中心會嘗試連線到目標節點。 如果連線成功，將會載入解決方案延伸模組的 UI，而 Windows 管理中心會在左側流覽窗格中顯示該解決方案的工具。

如果您想要為未由上述預設連線類型定義的服務建立管理 GUI，例如網路交換器或電腦名稱稱無法探索的其他硬體，您可能會想要建立自己的解決方案延伸模組。

> [!NOTE]
> 不熟悉不同的延伸模組類型嗎？ 深入瞭解延伸模組[架構和擴充功能類型](understand-extensions.md)。

## <a name="prepare-your-environment"></a>準備您的環境

如果您還沒有這麼做，請安裝所有專案所需的相依性和全域必要條件來[準備您的環境](prepare-development-environment.md)。

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>使用 Windows Admin Center CLI 建立新的解決方案延伸模組 ##

安裝所有相依性之後，您就可以開始建立新的解決方案延伸模組。  建立或流覽至包含您專案檔的資料夾，開啟命令提示字元，並將該資料夾設定為工作目錄。  使用先前安裝的 Windows Admin Center CLI，以下列語法建立新的擴充功能：

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱（含空格） | ```Contoso Inc``` |
| ```{!Solution Name}``` | 您的方案名稱（含空格） | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | 您的工具名稱（含空格） | ```Manage Foo Works``` |

這裡提供一個範例用法：

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

這會使用您為解決方案指定的名稱，在目前的工作目錄內建立新的資料夾，將所有必要的範本檔案複製到您的專案中，並使用您的公司、解決方案和工具名稱來設定檔案。  

接下來，將目錄切換到剛才建立的資料夾，然後執行下列命令來安裝必要的本機相依性：

```
npm install
```

完成後，您就可以設定將新延伸模組載入 Windows 管理中心所需的所有專案。 

## <a name="add-content-to-your-extension"></a>將內容新增至您的擴充功能

既然您已使用 Windows 管理中心 CLI 建立擴充功能，就可以開始自訂內容。  如需您可以執行之動作的範例，請參閱下列指南：

- 新增[空白模組](guides/add-module.md)
- 新增[iFrame](guides/add-iframe.md)
- 建立[自訂連接提供者](guides/create-connection-provider.md)
- 修改[根導覽行為](guides/modify-root-navigation.md)
 
如需更多範例，請參閱我們的[GITHUB SDK 網站](https://aka.ms/wacsdk)：
-  [開發人員工具](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools)是功能完整的延伸模組，可以側載到 Windows 系統管理中心，而且包含豐富的範例功能和工具範例集合，可讓您在自己的延伸模組中流覽和使用。

## <a name="build-and-side-load-your-extension"></a>組建和側邊載入您的擴充功能

接下來，建立您的延伸模組並將其載入至 Windows 系統管理中心。  開啟命令視窗，將目錄變更為您的來原始目錄，然後您就可以開始建立。

* 建置，並搭配 gulp 使用：

    ```
    gulp build
    gulp serve -p 4201
    ```

請注意，您必須選擇目前未使用的連接埠。 請確定您嘗試使用的不是 Windows Admin Center 正在用來執行的連接埠。

您可在 Windows Admin Center 中附加由本機服務的專案，以將專案側載至 Windows Admin Center 的本機執行個體供測試。

* 在網頁瀏覽器中啟動 Windows Admin Center
* 開啟偵錯工具 (F12)
* 開啟主控台，並鍵入下列命令：

    ```
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   重新整理網頁瀏覽器

您的專案現在會顯示在 [工具] 清單中，名稱旁邊有 (已側載) 標示。

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>以不同版本的 Windows Admin Center SDK 為目標

使用 SDK 變更和平臺變更讓擴充功能保持最新狀態很容易。  瞭解如何以[不同版本](target-sdk-version.md)的 WINDOWS 管理中心 SDK 為目標。