---
title: 從 Windows Admin Center SDK 0.1 到 1.0 移轉
description: 本指南將協助您從 Windows Admin Center SDK 0.1 到 1.0 的版本移轉
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886469"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>從 Windows Admin Center SDK 0.1 到 1.0 移轉

>適用於：Windows Admin Center 預覽版

本指南將協助您從 Windows Admin Center SDK 0.1 到 1.0 的版本移轉。  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1.深入了解新的控制項，副檔名為開發人員指南

Windows Admin Center 1902年和更新版本包含**開發人員指南**延伸模組，您可以使用它來尋找控制項 （包括新的、 可用的控制項） 和案例，協助您建置您自己的延伸模組的範例。  開發人員指南會取代**開發人員工具**從舊版 SDK 的延伸模組。

### <a name="use-the-dev-guide-in-windows-admin-center"></a>在 Windows Admin Center 中使用開發人員指南

開發人員指南是可為 Windows Admin Center 版本 1902年方案及更新版本。  開發人員指南已預先安裝，但它必須透過設定啟用。

**啟用 Windows Admin Center 中的開發人員指南：**

* 開啟 Windows Admin Center （1902 或更新版本）
* 按一下 **設定**中視窗的右上角的圖示
* 選取 [**進階**] 索引標籤
* 底下*實驗金鑰*，按一下 **新增**
* 輸入新值```msft.sme.shell.devguide```上一個步驟所建立的空白欄位中
* 按一下 **儲存並重新載入**

**開啟 Windows Admin Center 開發人員指南：**

* 開啟 Windows Admin Center （1902 或更新版本）
* 按一下位於左上方顯示所有的解決方案類型下拉式清單
* 選取 **開發人員指南**解決方案 
    * 如果您沒有看到列出的解決方案，請確定您已啟用開發人員指南 （請參閱上一節），而且必須重新載入 Windows Admin Center。
* 選取其中一個索引標籤即可瀏覽開發人員指南的內容
    * **平台：** 包含的程式碼範例*管理身分*並*通知*案例
    * **控制項：** 包含每個 SDK 中可用的控制項的範例
    * **管道：** 包含可用的轉換子和格式器函式的範例
    * **樣式：** 包含 SDK 中可用的 CSS 樣式的範例
    * **MsftSme:** 包含範例和進階的案例的指引 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>瀏覽 GitHub 上的開發人員指南的原始程式碼

您可以瀏覽[原始程式碼](https://github.com/Microsoft/windows-admin-center-sdk/)尋找範例 HTML、 CSS 和 TypeScript 程式碼範例的 GitHub 上的開發人員指南 》。

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2.準備開發環境的最新的 SDK

安裝或更新 node.js 版本[10.15.1 LTS 或更新版本](https://nodejs.org/en/)。

更新為最新版本的 Windows Admin Center CLI:

[//]: # "npm 解除安裝-g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

這些版本中更新您全域的相依性：

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3.建立新的專案使用最新的 SDK

使用 Windows Admin Center CLI 來建立新的專案目標```next```版本 (SDK 1.0):

[//]: # "wac create--公司 ' Contoso inc.營運 '-' 管理 Foo Works'-實驗性的版本的工具"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

接下來，將目錄切換到剛才建立的資料夾，然後執行以安裝必要的本機相依性```npm install ```。

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4.修改現有的專案以使用最新的 SDK

重要事項：繼續之前，請備份您的專案。

修改中的下行```package.json```目標```next```版本 (SDK 1.0):

[//]: # "'@microsoft/windows-admin-center-sdk': '實驗'"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

然後執行```npm install```更新整個專案的參考。

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5.使用 SDK CLI 來修正常見的移轉問題

重要事項：繼續之前，請備份您的專案。

從您的專案根資料夾中，執行下列 CLI 命令上您的專案，以自動修復常見的移轉問題：

``` cmd
wac updateSeven --update
```

此 CLI 命令會自動解決下列問題：

* 重新產生 ```package-lock.json```
* 在 angular 編譯環境中的更新檔案：
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6.使用 SDK CLI 來了解常見的移轉問題

從您的專案根資料夾中，執行下列 CLI 命令來稽核您的專案，並找出必須以手動方式解決的常見移轉問題：

``` cmd
wac updateSeven --audit
```

這會在您的專案中找到下列問題的執行個體：

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>取代這些 sme 類別中的下列 CSS 類別的使用方式：

| 舊的 CSS 類別 | 新的 CSS 類別 |
| -- | -- |
| .auto-flex-size |  .sme-position-flex-auto |
| .border-all |  .sme 框線內凹-sm 和.sme-框線的色彩-基底-90 |
| .border-bottom |  .sme 框線-底部 sm 和.sme-border-bottom-color-base-90 |
| .border-horizontal |  .sme 框線-水平 sm 和.sme-border-horizontal-color-base-90 |
| .border-left |  .sme 框線左邊-sm 和.sme-border-left-color-base-90 |
| .border-right |  .sme 框線右邊-sm 和.sme-border-right-color-base-90 |
| .border-top |  .sme 框線-頂端 sm 和.sme-border-top-color-base-90 |
| .border-vertical |  .sme 框線-垂直 sm 和.sme-border-vertical-color-base-90 |
| .break-word |  .sme-arrange-ws-wrap |
| .btn |  .sme 按鈕 OR 按鈕 |
| .btn 主要 |  .sme button.sme 按鈕-主要 OR.button.sme-按鈕-主要 |
| .color-dark |  .sme-color-alt |
| .color-light |  .sme-color-base |
| .color-light-gray |  .sme-color-base-90 |
| .fixed-flex-size |  .sme-position-flex-none |
| .flex-layout |  .sme-arrange-stack-h OR .sme-arrange-stack-v |
| .font-bold |  .sme-font-emphasis1 |
| .highlight |  .sme-background-color-yellow |
| .horizontal |  .sme-arrange-stack-h |
| .no-scroll |  .sme-position-flex-auto |
| .nowrap |  .sme-arrange-stack-h OR .sme-arrange-stack-v |
| .relative |  .sme-layout-relative |
| .relative-center |  .sme-layout-absolute .sme-position-center |
| .reverse |  .sme-arrange-stack-reversed |
| .stretch-absolute |  .sme-layout-absolute .sme-position-inset-none |
| .stretch-fixed |  .sme-layout-fixed .sme-position-inset-none |
| .stretch-vertical |  .sme-position-stretch-v |
| .stretch-width |  .sme-position-stretch-h |
| .vertical |  .sme-arrange-stack-v |
| .vertical-scroll-only |  .sme-排列-溢位-隱藏-x sme 排列-溢位-自動-y |
| .wrap |  .sme-arrange-wrapstack-h OR .sme-arrange-wrapstack-v |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>取代這些 sme 元件中的下列元件的使用方式：

| 舊的元件 | 新元件 |
| -- | -- |
| .alert |  sme-alert |
| .alert-danger |  sme-alert |
| .breadCrumb |  sme-alert |
| .checkbox |  sme-form-field[type="checkbox"] |
| .combobox |  sme-form-field[type="select"] |
| .dashboard |  sme-layout-content-zone-padded sme-arrange-stack-h |
| .details-panel |  sme-property-grid |
| .details-panel-container |  sme-property-grid |
| .details-tab |  sme 屬性方格 OR sme 樞紐 |
| .details-wrapper |  sme-property-grid |
| .disabled |  sme-disabled |
| .form-buttons | sme-form-field |
| .form-control | sme-form-field |
| .form-controls | sme-form-field |
| .form-group | sme-form-field |
| .form-group-label | sme-form-field |
| .form-input | sme-form-field |
| .form-stretch | sme-form-field |
| .input-file | sme-form-field |
| .nav-tabs |  sme-pivot |
| .radio |  sme-form-field[type="radio"] |
| .required-clue | sme-form-field |
| .searchbox |  sme-form-field[type="search"] |
| .toggle-switch |  sme-form-field[type="toggle-switch"] |
| .tool-container |  sme 版面配置-內容區域或 sme 版面配置-內容-區域-填補 |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>這些的 CSS 類別已被取代，而且不受支援：

| 舊的類別 | 已過時 |
| -- | -- |
| .acceptable | （已過時） |
| .color-error | （已過時） |
| .color-info | （已過時） |
| .color-success | （已過時） |
| .color-warning | （已過時） |
| .delete-button | （已過時） |
| .details-content | （已過時） |
| .error-cover | （已過時） |
| .error 訊息 | （已過時） |
| .guided-pane-button | （已過時） |
| .header-container | （已過時） |
| .icon 到先贏 | （已過時） |
| .indent | （已過時） |
| .invalid | （已過時） |
| .item-list | （已過時） |
| .modal-scrollable | （已過時） |
| .multi-section | （已過時） |
| .no-action-bar | （已過時） |
| .overflow-margins | （已過時） |
| .overflow-tool | （已過時） |
| .progress-cover | （已過時） |
| .right 面板 | （已過時） |
| .rollup | （已過時） |
| .rollup-status | （已過時） |
| .rollup-title | （已過時） |
| .rollup-value | （已過時） |
| .searchbox-action-bar | （已過時） |
| .size-h-1 | （已過時） |
| .size-h-2 | （已過時） |
| .size-h-3 | （已過時） |
| .size-h-4 | （已過時） |
| .size-h-full | （已過時） |
| .size-h-half | （已過時） |
| .size-v-1 | （已過時） |
| .size-v-2 | （已過時） |
| .size-v-3 | （已過時） |
| .size-v-4 | （已過時） |
| .status-icon | （已過時） |
| .svg-16px | （已過時） |
| .table-indent | （已過時） |
| .table-sm | （已過時） |
| .thin | （已過時） |
| .tile | （已過時） |
| .tile 主體 | （已過時） |
| .tile-content | （已過時） |
| .tile-footer | （已過時） |
| .tile-header | （已過時） |
| .tile-layout | （已過時） |
| .tile-table | （已過時） |
| .toolbar | （已過時） |
| .tool-bar | （已過時） |
| .tool-header | （已過時） |
| .tool-header-box | （已過時） |
| .tool-pane | （已過時） |
| .usage-bar | （已過時） |
| .usage-bar-area | （已過時） |
| .usage-bar-background | （已過時） |
| .usage-bar-title | （已過時） |
| .usage-bar-value | （已過時） |
| .usage-chart | （已過時） |
| .usage 訊息 | （已過時） |
| .usage-message-area | （已過時） |
| .usage-message-title | （已過時） |
| .warning | （已過時） |
| .white 空間 | （已過時） |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7.了解並解決問題的可觀察的物件

### <a name="update--rxjs-function-use-for-observable-objects"></a>更新```rxjs```函式使用的可觀察的物件

這些是一些常見的函式名稱已變更，可能有其他人在您的專案。

* 更新```Observable.empty()```至 ```empty()```
* 更新```Observable.of()```至 ```of()```
* 更新```.switchMap()```至 ```.pipe(switchMap())```
* 更新```.map()```至 ```.pipe(map())```
* 更新```flatMap()```至 ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>解決發生執行階段問題```.map()```和```.filter()```可預見物件上的函式

如果編譯器無法正確識別```observable```物件的型別```.map()```並```.filter()```函式```array```物件可能會改為對應至您的物件，進而導致在執行階段錯誤。  請確定您的函式傳回```observable```物件，指定明確的資料類型，若要避免這個問題。

```any``` 並沒有傳回型別可以會造成這個問題，找出這些模式的程式碼：

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8.解決其他常見的問題

這些技巧有助解決其他常見的問題：

* 執行```ng lint --fix```來修正常見問題 lint
* 執行```gulp build```重複若要以累加方式修正問題，```gulp build```可以自動解決

## <a name="9-build-and-serve-your-project"></a>9.建置及提供您的專案

執行下列命令來建置及提供您的專案 (SDK 1.0) 的最新版本：

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10.開啟 Windows Admin Center 暗色調佈景主題

若要開啟 Windows Admin Center 版本暗色調佈景主題 1902年和更新版本，請遵循下列步驟：

* 開啟 Windows Admin Center （1902 或更新版本）
* 按一下 **設定**中視窗的右上角的圖示
* 選取 [**進階**] 索引標籤
* 底下*實驗金鑰*，按一下 **新增**
* 輸入新值```msft.sme.shell.personalization```上一個步驟所建立的空白欄位中
* 按一下 **儲存並重新載入**
* 設定現在會有新的索引標籤中，**個人化**。  選取此索引標籤
* 變更**色彩**到**深色模式 （預覽）**
