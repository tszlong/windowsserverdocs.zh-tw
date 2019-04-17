---
title: 從 Windows Admin Center SDK 0.1 到 1.0 移轉
description: 本指南將協助您將從 Windows Admin Center SDK 版本 0.1 到 1.0 移轉
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 10d7606a5c2a1ddb234af597f199b6779b0058d4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116582"
---
# 從 Windows Admin Center SDK 0.1 到 1.0 移轉

>適用於： Windows Admin Center 預覽版

本指南將協助您將從 Windows Admin Center SDK 版本 0.1 到 1.0 移轉。  

## 1.深入了解新的控制項副檔名的開發人員指南

Windows Admin Center 版本 1902年和更新版本包含**開發人員指南**延伸模組，您可以使用它來尋找 （包括新的可用的控制項） 的控制項和案例可協助您建置您自己的擴充功能的範例。  開發人員指南會取代的**開發人員工具**擴充功能，從較早版本的 SDK。

### 使用 Windows Admin Center 中的開發人員指南

開發人員指南是可用做為在 Windows Admin Center 版本 1902年解決方案和更新版本。  開發人員指南會預先安裝，但它需要透過設定啟用。

**啟用 Windows Admin Center 中的開發人員指南：**

* 開啟 Windows Admin Center （版本 1902 和更新版本）
* 按一下視窗右上角中的**設定**圖示
* 選取 [**進階**] 索引標籤
* 在 [*實驗金鑰*，按一下 [**新增**
* 輸入新值```msft.sme.shell.devguide```由上一個步驟所建立的空白欄位中
* 按一下 [**儲存並重新載入**

**Windows Admin Center 中開啟開發人員指南：**

* 開啟 Windows Admin Center （版本 1902 和更新版本）
* 按一下下拉式清單中左上角來顯示所有方案類型
* 選取 [**開發人員指南**方案 
    * 如果您沒有看到列出的方案，請確定您已啟用開發人員指南 （請參閱上述的區段） 並已重新載入 Windows Admin Center。
* 選取其中一個索引標籤瀏覽內容的開發人員指南
    * **登陸：** 包含*做為管理*和*通知*案例的程式碼範例
    * **控制項：** 包含每個可用的控制項 SDK 中的範例
    * **管道：** 包含可用的轉換器和格式子函式的範例
    * **樣式：** 包含 CSS 樣式 SDK 中提供的範例
    * **MsftSme:** 包含範例以及指導方針進階案例 

### 瀏覽 GitHub 上的開發人員指南的原始碼

您可以瀏覽來尋找範例 HTML、 CSS 和 TypeScript 程式碼範例 GitHub 上的開發人員指南的[原始碼](https://github.com/Microsoft/windows-admin-center-sdk/)。

## 2.針對最新的 SDK 準備您的開發環境

安裝或更新版本 node.js [10.15.1 LTS 或更新版本](https://nodejs.org/en/)。

更新至最新版的 Windows Admin Center CLI:

[//]: # "npm 解除安裝-g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

更新您的全域相依性到這些版本：

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## 3.建立新的專案使用最新的 SDK

使用 Windows Admin Center CLI 建立新的專案目標為```next```版本 (SDK 1.0):

[//]: # "建立 wac： 公司 'Contoso Inc':' 管理 Foo 運作 ': 版本實驗性工具"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

接下來，將目錄變更至剛建立的資料夾，然後執行安裝所需的本機相依性```npm install ```。

## 4.修改現有的專案以使用最新的 SDK

重要： 繼續之前進行備份您的專案。

修改下列行中的```package.json```到目標```next```版本 (SDK 1.0):

[//]: # "'@microsoft/windows-admin-center-sdk': '實驗性'"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

然後執行```npm install```要更新您的專案參考。

## 5.使用 SDK CLI，修正常見移轉問題

重要： 繼續之前進行備份您的專案。

從您的專案的根資料夾，執行下列 CLI 命令在您的專案，自動修正常見移轉問題：

``` cmd
wac updateSeven --update
```

此 CLI 命令自動解決下列問題：

* 重新產生 ```package-lock.json```
* 在 angular 編譯環境中更新檔案：
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## 6.使用 SDK CLI，了解移轉常見問題

從您的專案的根資料夾，請執行下列 CLI 命令稽核您的專案，並尋找，必須以手動方式問題的常見移轉問題：

``` cmd
wac updateSeven --audit
```

這將會在您的專案中找到下列問題的執行個體：

### 使用這些 sme 類別來取代下列 CSS 類別的使用方式：

| 舊 CSS 類別 | 新的 CSS 類別 |
| -- | -- |
| .auto 彈性大小 |  .sme 位置-彈性自動 |
| .border 全部 |  .sme 框線-內凹 sm 和.sme-框線的色彩-基底-90 |
| .border 底部 |  .sme 框線-底部 sm 和.sme-border-bottom-color-base-90 |
| .border 水平 |  .sme 框線-水平 sm 和.sme-border-horizontal-color-base-90 |
| .border 左 |  .sme 框線-左 sm 和.sme-border-left-color-base-90 |
| .border 向右 |  .sme 框線-右 sm 和.sme-border-right-color-base-90 |
| .border 頂端 |  .sme 框線-頂端 sm 和.sme-border-top-color-base-90 |
| .border 垂直 |  .sme 框線-垂直 sm 和.sme-border-vertical-color-base-90 |
| .break word |  .sme-排列-ws-自動換行 |
| .btn |  .sme 按鈕或按鈕 |
| .btn 主要 |  .sme button.sme-按鈕主要 OR.button.sme-按鈕-主要 |
| .color 深色 |  alt 色彩.sme 鍵 |
| .color 光線 |  基底色彩.sme- |
| .color 淺灰色 |  .sme 色彩基底-90 |
| .fixed 彈性大小 |  .sme-位置-彈性-無 |
| .flex 版面配置 |  .sme-排列-堆疊-h 或.sme-排列-堆疊-v |
| .font 粗體 |  .sme-字型-emphasis1 |
| .highlight |  .sme 背景色彩-黃色 |
| .horizontal |  .sme-排列-堆疊-h |
| .no 捲軸 |  .sme 位置-彈性自動 |
| .nowrap |  .sme-排列-堆疊-h 或.sme-排列-堆疊-v |
| .relative |  .sme 相對版面配置 |
| .relative 中心 |  .sme-版面配置-絕對.sme 位置中心 |
| .reverse |  .sme-排列-堆疊-反轉 |
| .stretch 絕對 |  .sme-版面配置-絕對.sme-位置-內凹-無 |
| .stretch 固定 |  .sme 配置固定.sme-位置-內凹-無 |
| .stretch 垂直 |  .sme 位置 stretch-v |
| .stretch 寬度 |  .sme 位置 stretch-h |
| .vertical |  .sme-排列-堆疊-v |
| 僅.vertical 捲軸 |  .sme-排列-溢位-隱藏-sme 與排列-溢位-自動-x-y |
| .wrap |  .sme-排列-wrapstack-h 或.sme-排列-wrapstack-v |

### 使用這些 sme 元件來取代下列元件的用法：

| 舊的元件 | 新的元件 |
| -- | -- |
| .alert |  sme 警示 |
| .alert 危險 |  sme 警示 |
| .breadCrumb |  sme 警示 |
| .checkbox |  sme 表單欄位 [類型 ="核取方塊 」] |
| .combobox |  sme 表單欄位 [類型 = 「 選取 」] |
| .dashboard |  sme 版面配置-內容-區域-填補 sme-排列-堆疊-h |
| .details 面板 |  sme 屬性方格 |
| .details 面板容器 |  sme 屬性方格 |
| .details 索引標籤 |  sme 屬性方格 OR sme 樞紐 |
| .details 包裝函式 |  sme 屬性方格 |
| .disabled |  sme 停用 |
| 表單按鈕 | sme 表單欄位 |
| 表單控制項 | sme 表單欄位 |
| 表單控制項 | sme 表單欄位 |
| 表單群組 | sme 表單欄位 |
| 表單群組標籤 | sme 表單欄位 |
| 表單輸入 | sme 表單欄位 |
| 表單 stretch | sme 表單欄位 |
| .input 檔案 | sme 表單欄位 |
| .nav 索引標籤 |  sme 樞紐 |
| .radio |  sme 表單欄位 [類型 = 「 選項 」] |
| .required 線索 | sme 表單欄位 |
| .searchbox |  sme 表單欄位 [類型 = 「 搜尋 」] |
| .toggle 交換器 |  sme 表單欄位 [類型 ="切換開關 」] |
| .tool 容器 |  sme 版面配置-內容區域或 sme 版面配置-內容-區域-填補 |

### 這些 CSS 類別已過時，並已不再支援：

| 舊類別 | 已取代。 |
| -- | -- |
| .acceptable | （已過時） |
| .color 錯誤 | （已過時） |
| .color 資訊 | （已過時） |
| .color 成功 | （已過時） |
| .color 警告 | （已過時） |
| .delete 按鈕 | （已過時） |
| .details 內容 | （已過時） |
| .error 鍵盤保護蓋 | （已過時） |
| .error 訊息 | （已過時） |
| .guided 窗格按鈕 | （已過時） |
| .header 容器 | （已過時） |
| .icon win | （已過時） |
| .indent | （已過時） |
| .invalid | （已過時） |
| .item 清單 | （已過時） |
| .modal 可捲動 | （已過時） |
| 。 多區段 | （已過時） |
| .no 動作列 | （已過時） |
| .overflow 邊界 | （已過時） |
| .overflow 工具 | （已過時） |
| .progress 鍵盤保護蓋 | （已過時） |
| .right 面板 | （已過時） |
| .rollup | （已過時） |
| .rollup 狀態 | （已過時） |
| .rollup 標題 | （已過時） |
| .rollup 值 | （已過時） |
| .searchbox 動作列 | （已過時） |
| .size-h-1 | （已過時） |
| .size-h-2 | （已過時） |
| .size-h-3 | （已過時） |
| .size-h-4 | （已過時） |
| .size-h-完整 | （已過時） |
| .size-h-半 | （已過時） |
| .size-v-1 | （已過時） |
| .size-v-2 | （已過時） |
| .size-v-3 | （已過時） |
| .size-v-4 | （已過時） |
| .status 圖示 | （已過時） |
| .svg 16px | （已過時） |
| .table 縮排 | （已過時） |
| .table sm | （已過時） |
| .thin | （已過時） |
| .tile | （已過時） |
| .tile 主體 | （已過時） |
| .tile 內容 | （已過時） |
| .tile 頁尾 | （已過時） |
| .tile 標頭 | （已過時） |
| .tile 版面配置 | （已過時） |
| .tile 表格 | （已過時） |
| .toolbar | （已過時） |
| .tool 列 | （已過時） |
| .tool 標頭 | （已過時） |
| .tool 標頭方塊 | （已過時） |
| .tool 窗格 | （已過時） |
| .usage 列 | （已過時） |
| .usage 列區域 | （已過時） |
| .usage 列背景 | （已過時） |
| .usage 標題列 | （已過時） |
| .usage 列值 | （已過時） |
| .usage 圖表 | （已過時） |
| .usage 訊息 | （已過時） |
| .usage 訊息區域 | （已過時） |
| .usage 訊息標題 | （已過時） |
| .warning | （已過時） |
| .white 空間 | （已過時） |

## 7.了解和解決問題可觀察的物件

### 更新```rxjs```函式用於可觀察的物件

以下是一些常見的函式名稱已經變更，可能有其他人在您的專案。

* 更新```Observable.empty()```以 ```empty()```
* 更新```Observable.of()```以 ```of()```
* 更新```.switchMap()```以 ```.pipe(switchMap())```
* 更新```.map()```以 ```.pipe(map())```
* 更新```flatMap()```以 ```mergeMap()```


### 解決的問題執行階段```.map()```和```.filter()```可觀察的物件上的函式

如果編譯器無法正確地識別```observable```物件的類型```.map()```和```.filter()```函式，從```array```物件可能會改為對應至您的物件，導致在執行階段錯誤。  請確定您的函式會傳回```observable```物件，以指定明確的資料類型，若要避免這個問題。

```any``` 並沒有傳回類型可以會導致這個問題，尋找這些模式的程式碼：

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## 8.解決其他常見的問題

這些技術可協助解決其他常見的問題：

* 執行```ng lint --fix```修正不常見的問題
* 執行```gulp build```重複要以遞增方式修正問題，```gulp build```可以自動解析

## 9.建置及提供您的專案

執行下列命令來建置，並提供您的專案使用最新版本 (SDK 1.0):

``` cmd
gulp build
gulp serve --port 4201
```

## 10.開啟 Windows Admin Center 中的深色佈景主題

若要開啟 Windows Admin Center 版本中的深色佈景主題 1902年和更新版本，請依照下列步驟：

* 開啟 Windows Admin Center （版本 1902 和更新版本）
* 按一下視窗右上角中的**設定**圖示
* 選取 [**進階**] 索引標籤
* 在 [*實驗金鑰*，按一下 [**新增**
* 輸入新值```msft.sme.shell.personalization```由上一個步驟所建立的空白欄位中
* 按一下 [**儲存並重新載入**
* 設定現在會有新的索引標籤，**個人化**。  選取此索引標籤
* 將**色彩**變更為**深色模式 （預覽）**
