---
title: 開發工具擴充功能
description: 開發 Windows Admin Center SDK （專案檀香山） [工具] 延伸模組
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 092a97c1166f1090dd7c556f1ab86d42a1f46ee4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889269"
---
# <a name="develop-a-tool-extension"></a>開發工具擴充功能

>適用於：Windows Admin Center，Windows Admin Center 預覽

[工具] 延伸模組是與 Windows Admin Center 來管理連線，例如伺服器或叢集的使用者互動的主要方式。 當您按一下 Windows Admin Center 主畫面中的連接，並連接時，您接著會看到一份在左側的導覽窗格中的工具。 當您按一下一個工具時，工具延伸模組會載入，並顯示在右窗格中。

[工具] 延伸模組載入時，它可以在目標伺服器或叢集上執行 WMI 呼叫或 PowerShell 指令碼和在 UI 中顯示資訊或執行根據使用者輸入的命令。 工具延伸模組會定義哪些解決方案，它應該顯示，因而導致不同的每個解決方案的工具組。

> [!NOTE]
> 不熟悉不同的擴充功能類型嗎？ 深入了解[擴充性架構和擴充功能類型](understand-extensions.md)。

## <a name="prepare-your-environment"></a>準備您的環境

如果您還[準備您的環境](prepare-development-environment.md)藉由安裝相依性和所需的所有專案通用的必要條件。

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>使用 Windows Admin Center CLI 建立新的 [工具] 延伸模組 ##

一旦您已安裝的所有相依性，您就準備好建立您新的 [工具] 延伸模組。  建立或瀏覽至包含您的專案檔案的資料夾，開啟命令提示字元，並將該資料夾設為工作目錄。  使用先前已安裝的 Windows Admin Center CLI，請使用下列語法建立新的延伸模組：

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱 （含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 您的工具名稱 （含空格） | ```Manage Foo Works``` |

這裡提供一個範例用法：

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

這會建立新的資料夾，在目前工作目錄中使用的名稱指定為您的工具，將所有必要範本檔案複製到您的專案，並會設定檔案，以您的公司和工具的名稱。  

接下來，將目錄切換到剛才建立的資料夾，然後執行下列命令來安裝必要的本機相依性：

``` cmd
npm install
```

完成之後，您已設定您需要將您新的擴充功能載入 Windows Admin Center 的所有項目。 

## <a name="add-content-to-your-extension"></a>將內容新增至您的延伸模組

既然您已使用 Windows Admin Center CLI 來建立擴充功能，您準備自訂內容。  如需範例，您可以執行的動作，請參閱這些指南：

- 新增[空的模組](guides\add-module.md)
- 新增[iFrame](guides\add-iframe.md)
 
可以找到更多範例我們[GitHub SDK 網站](https://aka.ms/wacsdk):
-  [開發人員工具](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools)是全功能的延伸模組可以側載到 Windows Admin Center，並包含豐富的功能和工具範例，您可以瀏覽，並使用您自己的延伸模組中的集合。

## <a name="customize-your-extensions-icon"></a>自訂您的延伸模組圖示

您可以自訂您的擴充功能，工具清單中顯示的圖示。  若要這樣做，請修改所有```icon```中的項目```manifest.json```您的擴充功能：

``` json
"icon": "{!icon-uri}",
```

| 值 | 說明 | 範例 uri |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | 您的圖示資源的位置 | ```assets/foo-icon.svg``` |

注意：目前，自訂圖示不側邊載入您的延伸模組，在開發模式時才會顯示。  因應措施，移除的內容```target```，如下所示：

``` json
"target": "",
```

此設定是唯一有效的側載在開發模式中，因此請務必保留中包含的值```target```然後將它還原之前發行您的延伸模組。

## <a name="build-and-side-load-your-extension"></a>組建和側邊載入您的延伸模組

接下來，建置和側邊載入您的擴充 Windows Admin Center。  開啟命令視窗，將目錄變更為您的來源目錄，則您可以開始建置。

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

保持最新的 SDK 變更與平台變更您的延伸模組很簡單。  了解如何[不同的版本為目標](target-sdk-version.md)的 Windows Admin Center sdk。