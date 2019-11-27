---
title: 從 Windows Admin Center SDK 0.1 遷移至1。0
description: 本指南可協助您從 Windows Admin Center SDK 0.1 版遷移至1。0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c52870178e7caff0abc8ddcccc62966d637dd3c9
ms.sourcegitcommit: ac9946deb4fa70203a9b05e0386deb4244b8ca55
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "71357055"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>從 Windows Admin Center SDK 0.1 遷移至1。0

>適用于： Windows 管理中心預覽

本指南可協助您從 Windows Admin Center SDK 0.1 版遷移至1.0。  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1. 使用 Dev Guide 延伸模組瞭解新的控制項

Windows 管理中心1902版和更新版本包含**Dev Guide**延伸模組，可讓您用來尋找控制項的範例（包括新的控制項）和案例，以協助您建立自己的延伸模組。  Dev Guide 會取代舊版 SDK 的**開發人員工具**擴充功能。

### <a name="use-the-dev-guide-in-windows-admin-center"></a>使用 Windows 系統管理中心的開發指南

《開發人員指南》以 Windows 系統管理中心1902版和更新版本中的解決方案形式提供。  開發人員指南已預先安裝，但必須透過 [設定] 加以啟用。

**在 Windows 系統管理中心啟用開發人員指南：**

* 開啟 Windows 系統管理中心（版本1902和更新版本）
* 按一下視窗右上角的 [**設定**] 圖示
* 選取 [ **Advanced** ] 索引標籤
* 在 [*實驗金鑰*] 底下，按一下 [**新增**]
* 在上一個步驟所建立的空白欄位中，輸入新的值 ```msft.sme.shell.devguide```
* 按一下 [**儲存並重載**]

**在 Windows 系統管理中心內開啟開發人員指南：**

* 開啟 Windows 系統管理中心（版本1902和更新版本）
* 按一下左上方的下拉式顯示所有解決方案類型
* 選取**Dev Guide**解決方案 
    * 如果您沒有看到列出的解決方案，請確定您已啟用開發人員指南（請參閱上面的一節），並已重載 Windows 系統管理中心。
* 選取其中一個索引標籤，以流覽 Dev Guide 的內容
    * **登陸：** 包含用於*管理 As*和*通知*案例的程式碼範例
    * **控制項：** 包含 SDK 中每個可用控制項的範例
    * **管道：** 包含可用的轉換器和格式器函式的範例
    * **樣式：** 包含 SDK 中可用的 CSS 樣式範例
    * **MsftSme：** 包含 advanced 案例的範例和指引 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>流覽 GitHub 上的開發人員指南原始程式碼

您可以流覽 GitHub 上的《開發人員指南》[原始程式碼](https://github.com/Microsoft/windows-admin-center-sdk/)，以尋找範例 HTML、CSS 和 TypeScript 程式碼範例。

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2. 準備適用于最新 SDK 的開發環境

安裝或更新 node.js version [10.15.1 LTS](https://nodejs.org/en/)（含）以後版本。

將 Windows Admin Center CLI 更新為最新版本：

[//]: # "npm 卸載-g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

將您的全域相依性更新為下列版本：

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3. 使用最新的 SDK 建立新的專案

使用 Windows Admin Center CLI 建立以 ```next``` 版本為目標的新專案（SDK 1.0）：

[//]: # "wac 建立--公司 ' Contoso Inc. '--工具 ' 管理 Foo Works '--實驗版本"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

接下來，將目錄變更為剛建立的資料夾，然後執行 ```npm install ```來安裝必要的本機相依性。

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4. 修改現有的專案以使用最新的 SDK

重要事項：請先備份您的專案，再繼續進行。

修改 ```package.json``` 中的下面這一行，以 ```next``` 版本（SDK 1.0）為目標：

[//]: # "'@microsoft/windows-admin-center-sdk'： ' 實驗性 '"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

然後執行 ```npm install``` 以更新整個專案中的參考。

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5. 使用 SDK CLI 來修正常見的遷移問題

重要事項：請先備份您的專案，再繼續進行。

從專案的根資料夾中，對您的專案執行下列 CLI 命令，以自動修正一般的遷移問題：

``` cmd
wac updateSeven --update
```

此 CLI 命令會自動解決下列問題：

* 重新產生 ```package-lock.json```
* 在「角度編譯」環境中更新檔案：
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6. 使用 SDK CLI 來瞭解常見的遷移問題

從專案的根資料夾，執行下列 CLI 命令來審查您的專案，並尋找需要手動解決的常見遷移問題：

``` cmd
wac updateSeven --audit
```

這會在您的專案中找到下列問題的實例：

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>將下列 CSS 類別的使用方式取代為這些 sme 類別：

| 舊的 CSS 類別 | 新增 CSS 類別 |
| -- | -- |
| 。自動調整大小 |  。 sme-位置-彈性-自動 |
| . 框線-全部 |  。 sme-框線-中凹--------------------90 |
| . 框線-下 |  . sme-框線-底端-sm 和.--底端-右下角-90 |
| 。框線-水準 |  . sme-框線-水準-sm 和.-框線-水準-------90 |
| 。框線-左方 |  . sme-左邊緣----------左-----左邊-90 |
| 。框線-right |  . sme-框線-右------右---右下角-90 |
| 。框線-頂端 |  。 sme-框線-90 頂尖-----top-- |
| . 框線-垂直 |  . sme-框線-垂直-sm 和.-框線-垂直--------------90 |
| . break-單字 |  。 sme-排列-ws-addressing |
| . btn |  . sme-按鈕或按鈕 |
| . btn-primary |  sme-button。 sme-按鈕-主要或按鈕。 sme-按鈕-主要 |
| 。 color-深色 |  . sme-color-alt |
| 。 color-light |  . sme-color-base |
| 。 color-淺灰色 |  . sme-基色-90 |
| 。固定-彈性-大小 |  . sme-位置-彈性-無 |
| 。 flex-版面配置 |  。 sme-排列堆疊-h 或。 |
| 。字型-粗體 |  . sme-font-size-emphasis1 |
| 。反白顯示 |  . sme-背景-彩色-黃色 |
| 。水準 |  。 sme-排列-堆疊-h |
| 。 no-scroll |  。 sme-位置-彈性-自動 |
| .nowrap |  。 sme-排列堆疊-h 或。 |
| 。相對 |  。 sme-版面配置-相對 |
| 。相對中心 |  。 sme-版面配置-絕對. sme-位置-中心 |
| 。反轉 |  。 sme-排列-堆疊-反轉 |
| . stretch-絕對 |  . sme-layout-絕對. sme-位置-中-無 |
| 。 stretch-固定 |  。 sme-版面配置-已修正。 sme-位置-內凹-無 |
| . stretch-垂直 |  。 sme-位置-延展-v |
| 。 stretch-寬度 |  。 sme-位置-延展-h |
| . 垂直 |  。 sme-排列-堆疊-v |
| 。垂直-僅限滾動 |  。 sme-排列-溢位-hide-x sme-排列-溢位-auto-y |
| wrap |  wrapstack-h 或 wrapstack-v- |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>以這些 sme 元件取代下列元件的使用方式：

| 舊元件 | 新增元件 |
| -- | -- |
| 。警示 |  sme-警示 |
| 。警示-危險 |  sme-警示 |
| . 階層連結 |  sme-警示 |
| . checkbox |  sme-表單欄位 [type = "checkbox"] |
| . combobox |  sme-表單-欄位 [type = "select"] |
| 。儀表板 |  sme-版面配置-內容區域-填補的 sme-排列堆疊-h |
| 。詳細資料-面板 |  sme-屬性方格 |
| 。詳細資料-面板-容器 |  sme-屬性方格 |
| 。詳細資料-索引標籤 |  sme-屬性方格或 sme-pivot |
| 。詳細資料-包裝函式 |  sme-屬性方格 |
| 。已停用 |  sme-已停用 |
| 。表單按鈕 | sme-表單欄位 |
| 。表單控制項 | sme-表單欄位 |
| 。表單控制項 | sme-表單欄位 |
| . 表單群組 | sme-表單欄位 |
| 。表單-群組-標籤 | sme-表單欄位 |
| 。表單-輸入 | sme-表單欄位 |
| 。表單-延展 | sme-表單欄位 |
| . input-file | sme-表單欄位 |
| . nav-索引標籤 |  sme-pivot |
| . 收音機 |  sme-表單欄位 [type = "選項按鈕]] |
| 。必要-線索 | sme-表單欄位 |
| . 搜尋方塊 |  sme-表單-欄位 [類型 = "搜尋]] |
| 。切換-切換 |  sme-表單-欄位 [type = "切換切換]] |
| 。工具-容器 |  sme-版面配置-內容-區域或 sme-版面配置-內容-區域-填補 |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>這些 CSS 類別已被取代，不再受到支援：

| 舊類別 | 已過時 |
| -- | -- |
| 。可接受 | 不再 |
| 。 color-錯誤 | 不再 |
| 。 color-info | 不再 |
| 。 color-成功 | 不再 |
| 。 color-警告 | 不再 |
| 。刪除-按鈕 | 不再 |
| 。詳細資料-內容 | 不再 |
| 。錯誤-涵蓋 | 不再 |
| 。錯誤-訊息 | 不再 |
| 。引導式窗格按鈕 | 不再 |
| . header-容器 | 不再 |
| 。圖示-win | 不再 |
| 。縮排 | 不再 |
| 。無效 | 不再 |
| . item-list | 不再 |
| 。強制回應-可滾動 | 不再 |
| 。多個區段 | 不再 |
| 。無動作-橫條圖 | 不再 |
| 。溢位-邊界 | 不再 |
| . 溢位工具 | 不再 |
| 。進度-涵蓋範圍 | 不再 |
| 。右面板 | 不再 |
| rollup | 不再 |
| rollup-狀態 | 不再 |
| rollup-標題 | 不再 |
| rollup-值 | 不再 |
| . 搜尋方塊-動作-橫條 | 不再 |
| 。大小-h-1 | 不再 |
| 。大小-h-2 | 不再 |
| 。大小-h-3 | 不再 |
| 。大小-h-4 | 不再 |
| 。大小-h-完整 | 不再 |
| 。大小-h-半 | 不再 |
| 。大小-v-1 | 不再 |
| 。大小-v-2 | 不再 |
| 。大小-v-3 | 不再 |
| 。大小-v-4 | 不再 |
| 。狀態-圖示 | 不再 |
| svg-16px | 不再 |
| 。資料表-縮排 | 不再 |
| 。 table-sm | 不再 |
| . 精簡 | 不再 |
| . 磚 | 不再 |
| . 磚-主體 | 不再 |
| . 磚-內容 | 不再 |
| . 磚-頁尾 | 不再 |
| . 磚-標頭 | 不再 |
| 。磚-版面配置 | 不再 |
| . 磚-資料表 | 不再 |
| . 工具列 | 不再 |
| . 工具列 | 不再 |
| 。工具-標頭 | 不再 |
| 。工具-頁首-box | 不再 |
| 。工具-窗格 | 不再 |
| 。使用方式-橫條圖 | 不再 |
| 。使用方式-橫條圖-區域 | 不再 |
| 。使用方式-橫條-背景 | 不再 |
| 。使用方式-橫條-標題 | 不再 |
| 。使用量-橫條-值 | 不再 |
| 。使用方式-圖表 | 不再 |
| 。使用方式-訊息 | 不再 |
| 。使用方式-訊息區域 | 不再 |
| 。使用方式-訊息-標題 | 不再 |
| 。警告 | 不再 |
| 。空白字元 | 不再 |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7. 瞭解並解決可觀察物件的問題

### <a name="update--rxjs-function-use-for-observable-objects"></a>更新可觀察物件的 ```rxjs``` 函數使用

這些是一些已變更的常見函式名稱，而您的專案中可能會有其他功能。

* 將 ```Observable.empty()``` 更新為 ```empty()```
* 將 ```Observable.of()``` 更新為 ```of()```
* 將 ```.switchMap()``` 更新為 ```.pipe(switchMap())```
* 將 ```.map()``` 更新為 ```.pipe(map())```
* 將 ```flatMap()``` 更新為 ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>解決可觀察物件上 ```.map()``` 和 ```.filter()``` 函數的執行時間問題

如果編譯器無法正確識別 ```observable``` 物件的型別，則來自 ```array``` 物件的 ```.map()``` 和 ```.filter()``` 函式可能會對應到您的物件，而導致執行時間發生錯誤。  請確定您的函式傳回的 ```observable``` 物件指定明確的資料類型，以避免發生此問題。

```any```，而且沒有傳回類型可能會造成此問題，請尋找具有下列模式的程式碼：

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8. 解決其他常見的問題

這些技術可協助解決其他常見問題：

* 執行 ```ng lint --fix``` 以修正常見不起毛的問題
* 重複執行 ```gulp build```，以累加方式修正 ```gulp build``` 可以自動解決的問題

## <a name="9-build-and-serve-your-project"></a>9. 建立和服務您的專案

執行下列命令，以使用最新版本來建立和服務您的專案（SDK 1.0）：

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10. 在 Windows 系統管理中心開啟深色主題

若要在 Windows Admin Center 1902 版和更新版本中開啟深色主題，請遵循下列步驟：

* 開啟 Windows 系統管理中心（版本1902和更新版本）
* 按一下視窗右上角的 [**設定**] 圖示
* 選取 [ **Advanced** ] 索引標籤
* 在 [*實驗金鑰*] 底下，按一下 [**新增**]
* 在上一個步驟所建立的空白欄位中，輸入新的值 ```msft.sme.shell.personalization```
* 按一下 [**儲存並重載**]
* 設定現在會有新的索引標籤 [**個人**化]。  選取此索引標籤
* 將**色彩**變更為**深色模式（預覽）**
