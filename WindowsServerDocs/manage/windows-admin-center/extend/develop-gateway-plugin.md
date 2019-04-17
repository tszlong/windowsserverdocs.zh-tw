---
title: 開發閘道外掛程式
description: 開發閘道外掛程式 Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cee5b8e3611a264119947103d22d9aa3b9a56b
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081155"
---
# 開發閘道外掛程式

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows Admin Center 閘道外掛程式啟用 API 通訊從 UI 工具或方案的目標節點。  Windows Admin Center 裝載轉送要在目標節點上執行的命令和指令碼從閘道外掛程式的閘道服務。 閘道服務可以延伸到包含自訂閘道外掛程式支援預設樣式以外的通訊協定。

使用 Windows Admin Center 預設會包含這些閘道外掛程式：

* PowerShell 閘道外掛程式
* WMI 閘道外掛程式

如果您想要使用 PowerShell 或 WMI 以外的通訊協定進行通訊，這類的其餘部分，您可以建置您自己的閘道外掛程式。  閘道外掛程式載入到個別的 AppDomain 從現有的閘道程序，但使用權限提高權限的相同層級。

> [!NOTE]
> 使用不同的擴充功能類型不熟悉？ 深入了解[擴充性架構和擴充功能類型](understand-extensions.md)。

## 準備您的環境

如果您還沒有這樣做，[您的環境準備好](prepare-development-environment.md)安裝相依性與全域適用於所有專案所需的必要條件。

## 建立閘道外掛程式 （C# 程式庫）

若要建立自訂的閘道外掛程式，建立新 C# 實作的類別```IPlugIn```介面從```Microsoft.ManagementExperience.FeatureInterfaces```命名空間。  

> [!NOTE]
> ```IFeature```適用於較舊版本的 SDK，介面現在會標示為過時。  所有的閘道外掛程式開發應該使用 IPlugIn （或選擇性地 HttpPlugIn 抽象類別）。

### 從 GitHub 下載範例

若要快速開始使用自訂的閘道外掛程式，您可以複製或下載[範例 C# 外掛程式專案](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)，從我們的 Windows Admin Center SDK [GitHub 網站](https://aka.ms/wacsdk)。

### 新增內容

將新的內容新增至您複製的[範例外掛程式專案 C#](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)專案 （或您自己的專案） 包含您自訂的 Api，複製，然後建置您的自訂閘道外掛程式 DLL 檔案，在下一個步驟中使用。

### 部署適用於測試的外掛程式

測試您自訂的閘道外掛程式 DLL 載入 Windows Admin Center 閘道程序。

Windows Admin Center 中的所有外掛程式會都尋找```plugins```目前的電腦 （使用 Environment.SpecialFolder 列舉 CommonApplicationData 值） 的應用程式資料資料夾中的資料夾。 此位置是在 Windows 10 上```C:\ProgramData\Server Management Experience```。  如果```plugins```資料夾不存在，但您可以自行建立資料夾。

> [!NOTE]
> 您可以藉由更新 」 StaticsFolder 」 設定值來覆寫中偵錯組建的外掛程式位置。 如果您要在本機偵錯，這項設定會處於桌面解決方案的 App.Config。 

在外掛程式資料夾 (在此範例中， ```C:\ProgramData\Server Management Experience\plugins```)

* 建立新的資料夾具有相同名稱做為```Name```的屬性值```Feature```中您自訂的閘道外掛程式 DLL (在我們的範例專案，```Name```是 「 範例 Uno 」)
* 將您自訂的閘道外掛程式的 DLL 檔案複製到這個新的資料夾
* 重新啟動 Windows Admin Center 程序

Windows 系統管理員程序重新啟動之後，您將無法練習中您自訂的閘道外掛程式 DLL 的 Api 發出的 GET、 放置、 補充程式、 刪除或張貼到 ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### 選用： 附加至偵錯外掛程式

在 Visual Studio 2017 中，從 [偵錯] 功能表中，選取 「 附加至處理序 」。 在下一個視窗中，捲動可用處理程序清單並選取 SMEDesktop.exe，然後按一下 [「 附加 」。 一次在偵錯工具開始，您可以放置中斷點功能的程式碼與上述的 URL 格式透過然後練習中。 如我們的範例專案 (功能名稱: 「 範例 Uno 」) 的 url: 」http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno」

## 使用 Windows Admin Center CLI 建立工具擴充功能 ##

現在我們需要建立工具擴充功能，您可以從中呼叫您的自訂閘道外掛程式。  建立或瀏覽至資料夾，您想要用來儲存您的專案檔、 開啟命令提示字元中，以及設定該資料夾做為工作目錄。  使用先前已安裝 Windows Admin Center CLI，使用下列語法建立新的擴充功能：

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱 （以空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 您的工具名稱 （以空格） | ```Manage Foo Works``` |

這裡提供一個範例用法：

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

這會建立新的資料夾內的目前的工作目錄，使用您的名稱指定給您的工具、 會將所有必要的範本檔案複製到您的專案，並使用您的公司和工具的名稱，來設定檔案。  

接下來，將目錄變更至剛建立的資料夾，然後執行下列命令來安裝所需的本機相依性：

```
npm install
```

一旦完成後，您已經設定您要將 Windows Admin Center 載入您新的擴充功能的所有項目。 

## 連線到您的自訂閘道外掛程式的工具擴充功能

現在，您已經在使用 Windows Admin Center CLI 建立擴充功能，您已經準備好要連線到您的自訂閘道外掛程式，您的工具擴充功能，依照下列步驟：

- 新增一個[空的模組](guides\add-module.md)
- 在您的工具擴充功能中使用您的[自訂閘道外掛程式](guides\use-custom-gateway-plugin.md)
 
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