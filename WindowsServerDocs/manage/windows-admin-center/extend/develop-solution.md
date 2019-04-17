---
title: 開發解決方案擴充功能
description: 開發 Windows Admin Center SDK (Project Honolulu) 的解決方案擴充功能
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ed5ecddbaef91f127846825e408a9a6ec65ff741
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081065"
---
# 開發解決方案擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

解決方案主要定義的唯一要透過 Windows Admin Center 管理的物件類型。  這些解決方案/連線類型是預設包含於 Windows Admin Center:

* Windows Server 連線
* Windows 電腦連線
* 容錯移轉叢集連線
* 超融合式叢集連線

當您從 Windows Admin Center 連線頁面選取連線時，載入該連線類型的解決方案擴充功能，以及 Windows Admin Center 將會嘗試連線到目標節點。 如果是成功的連線，解決方案將會載入延伸模組的 UI，以及 Windows Admin Center 將會在左側瀏覽窗格中顯示該解決方案的工具。

如果您想要建置以管理 GUI 服務未定義的上方的預設連線類型、 這類的網路交換器或不搜尋其他硬體依電腦名稱，您可能會想要建立您自己的解決方案擴充功能。

> [!NOTE]
> 使用不同的擴充功能類型不熟悉？ 深入了解[擴充性架構和擴充功能類型](understand-extensions.md)。

## 準備您的環境

如果您還沒有這樣做，[您的環境準備好](prepare-development-environment.md)安裝相依性與全域適用於所有專案所需的必要條件。

## 使用 Windows Admin Center CLI 建立新的解決方案擴充功能 ##

一旦您已安裝的所有相依性，您已經準備好建立您新的解決方案擴充功能。  建立或瀏覽資料夾，其中包含您的專案檔、 開啟命令提示字元，並設定該資料夾做為工作目錄。  使用先前已安裝 Windows Admin Center CLI，使用下列語法建立新的擴充功能：

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱 （以空格） | ```Contoso Inc``` |
| ```{!Solution Name}``` | 您的方案名稱 （以空格） | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | 您的工具名稱 （以空格） | ```Manage Foo Works``` |

這裡提供一個範例用法：

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

這會建立新的資料夾內的目前的工作目錄，使用您的名稱指定給您的方案，會將所有必要的範本檔案複製到您的專案，並設定檔案與您的公司、 解決方案，工具名稱。  

接下來，將目錄變更至剛建立的資料夾，然後執行下列命令來安裝所需的本機相依性：

```
npm install
```

一旦完成後，您已經設定您要將 Windows Admin Center 載入您新的擴充功能的所有項目。 

## 將內容新增至您的擴充功能

現在，您已經在使用 Windows Admin Center CLI 建立擴充功能，您已經準備好自訂內容。  如需您可以執行的動作的範例，請參閱下列指南：

- 新增一個[空的模組](guides\add-module.md)
- 新增[iFrame](guides\add-iframe.md)
- 建立[自訂連線提供者](guides\create-connection-provider.md)
- 修改[根瀏覽行為](guides\modify-root-navigation.md)
 
我們的[GitHub SDK 網站](https://aka.ms/wacsdk)，您可以找到更多的範例：
-  [開發人員工具](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools)齊全的擴充功能，可側載到 Windows Admin Center，並包含豐富的範例功能與工具範例，您可以瀏覽，並使用您自己的延伸模組中的集合。

## 建置並側載您的擴充功能

接下來，建置並側載您的擴充功能至 Windows Admin Center。  開啟命令視窗、 將目錄變更為您的來源目錄，則您已準備好建置。

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

## 目標為不同版本的 Windows Admin Center SDK

讓您的擴充功能保持在最新的 SDK 的變更和平台變更非常容易。  閱讀有關的 Windows Admin Center SDK 的[不同版本為目標](target-sdk-version.md)。