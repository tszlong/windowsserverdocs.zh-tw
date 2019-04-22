---
title: 開發解決方案擴充功能
description: 開發解決方案的擴充功能 Windows Admin Center SDK （專案檀香山）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ed5ecddbaef91f127846825e408a9a6ec65ff741
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825469"
---
# <a name="develop-a-solution-extension"></a>開發解決方案擴充功能

>適用於：Windows Admin Center，Windows Admin Center 預覽

解決方案主要是定義唯一的一種您想要管理透過 Windows Admin Center 的物件。  這些解決方案/連線類型會隨附預設的 Windows Admin Center:

* Windows Server 的連線
* Windows 電腦連接
* 容錯移轉叢集的連線
* 超交集叢集連線

當您從 Windows Admin Center 的 [連接] 頁面中選取連接時，該連線類型的解決方案延伸模組已載入，和 Windows Admin Center 會嘗試連線至目標節點。 如果連線成功，解決方案會載入延伸模組的 UI，以及 Windows Admin Center 會在左側的導覽窗格中顯示為該方案的工具。

如果您想要建置未定義上述的預設連接類型、 這類網路交換器，或不到其他硬體的電腦名稱的服務管理 GUI，您可能想要建立您自己的解決方案擴充功能。

> [!NOTE]
> 不熟悉不同的擴充功能類型嗎？ 深入了解[擴充性架構和擴充功能類型](understand-extensions.md)。

## <a name="prepare-your-environment"></a>準備您的環境

如果您還[準備您的環境](prepare-development-environment.md)藉由安裝相依性和所需的所有專案通用的必要條件。

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>使用 Windows Admin Center CLI 建立新的方案延伸模組 ##

一旦您已安裝的所有相依性，您已準備好建立新的解決方案擴充功能。  建立或瀏覽至包含您的專案檔案的資料夾，開啟命令提示字元，並將該資料夾設為工作目錄。  使用先前已安裝的 Windows Admin Center CLI，請使用下列語法建立新的延伸模組：

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱 （含空格） | ```Contoso Inc``` |
| ```{!Solution Name}``` | 您的方案名稱 （含空格） | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | 您的工具名稱 （含空格） | ```Manage Foo Works``` |

這裡提供一個範例用法：

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

這會建立新的資料夾，在目前工作目錄中使用您指定為您的方案的名稱、 將所有必要範本檔案複製到您的專案，以您的公司、 解決方案和工具名稱設定檔案。  

接下來，將目錄切換到剛才建立的資料夾，然後執行下列命令來安裝必要的本機相依性：

```
npm install
```

完成之後，您已設定您需要將您新的擴充功能載入 Windows Admin Center 的所有項目。 

## <a name="add-content-to-your-extension"></a>將內容新增至您的延伸模組

既然您已使用 Windows Admin Center CLI 來建立擴充功能，您準備自訂內容。  如需範例，您可以執行的動作，請參閱這些指南：

- 新增[空的模組](guides\add-module.md)
- 新增[iFrame](guides\add-iframe.md)
- 建立[自訂連接提供者](guides\create-connection-provider.md)
- 修改[根導覽行為](guides\modify-root-navigation.md)
 
可以找到更多範例我們[GitHub SDK 網站](https://aka.ms/wacsdk):
-  [開發人員工具](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools)是全功能的延伸模組可以側載到 Windows Admin Center，並包含豐富的功能和工具範例，您可以瀏覽，並使用您自己的延伸模組中的集合。

## <a name="build-and-side-load-your-extension"></a>組建和側邊載入您的延伸模組

接下來，建置和側邊載入您的擴充 Windows Admin Center。  開啟命令視窗，將目錄變更為您的來源目錄，則您可以開始建置。

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

保持最新的 SDK 變更與平台變更您的延伸模組很簡單。  了解如何[不同的版本為目標](target-sdk-version.md)的 Windows Admin Center sdk。