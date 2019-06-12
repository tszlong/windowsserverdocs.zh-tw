---
title: 開發閘道外掛程式
description: 開發 Windows Admin Center SDK （專案檀香山） 的閘道外掛程式
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 66e36a349fc6bd38a77ccf4f00d380788ea4b422
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445951"
---
# <a name="develop-a-gateway-plugin"></a>開發閘道外掛程式

>適用於：Windows Admin Center，Windows Admin Center 預覽

Windows Admin Center 閘道外掛程式可讓 API 通訊從 UI 工具或方案的目標節點。  Windows Admin Center 裝載轉送命令和指令碼從閘道外掛程式来在目標節點上執行的閘道服務。 閘道服務可以擴充以包含自訂閘道外掛程式，可支援的預設值以外的通訊協定。

使用 Windows Admin Center 的預設會包含這些閘道外掛程式：

* PowerShell 閘道外掛程式
* WMI 閘道外掛程式

如果您想要使用 PowerShell 或 WMI 以外的通訊協定進行通訊，例如 rest 中，您可以建置自己的閘道外掛程式。  閘道外掛程式載入現有的閘道程序，從不同的 AppDomain，但使用相同的層級權限提高的權限。

> [!NOTE]
> 不熟悉不同的擴充功能類型嗎？ 深入了解[擴充性架構和擴充功能類型](understand-extensions.md)。

## <a name="prepare-your-environment"></a>準備您的環境

如果您還[準備您的環境](prepare-development-environment.md)藉由安裝相依性和所需的所有專案通用的必要條件。

## <a name="create-a-gateway-plugin-c-library"></a>建立閘道外掛程式 (C#程式庫)

若要建立自訂閘道外掛程式，建立新的C#類別可實作```IPlugIn```從介面```Microsoft.ManagementExperience.FeatureInterfaces```命名空間。  

> [!NOTE]
> ```IFeature```介面，可在舊版的 SDK，現在會標示為過時。  IPlugIn （或選擇性地 HttpPlugIn 抽象類別），則應該使用所有的閘道外掛程式開發。

### <a name="download-sample-from-github"></a>從 GitHub 下載範例

若要快速開始使用自訂閘道外掛程式，您可以複製或下載一份我們[範例C#外掛程式專案](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)從我們的 Windows Admin Center SDK [GitHub 網站](https://aka.ms/wacsdk)。

### <a name="add-content"></a>新增內容

將新的內容新增至您複製的複本[範例C#外掛程式專案](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)專案 （或您自己的專案） 包含您的自訂 Api，然後建置您的自訂閘道外掛程式 DLL 檔案用於下一個步驟。

### <a name="deploy-plugin-for-testing"></a>部署適用於測試外掛程式

載入 Windows Admin Center 閘道程序，以測試您的自訂閘道外掛程式 DLL。

中的所有外掛程式會都尋找 Windows Admin Center```plugins```目前的電腦 （使用 CommonApplicationData Environment.SpecialFolder 列舉值） 的應用程式資料資料夾中的資料夾。 此位置是在 Windows 10 上```C:\ProgramData\Server Management Experience```。  如果```plugins``` 資料夾尚未存在，您可以自行建立的資料夾。

> [!NOTE]
> 若要覆寫在偵錯組建中的外掛程式位置，可以更新 「 StaticsFolder"組態值。 如果您是解決方案的在本機偵錯，此設定是解決方案的在桌面的 App.Config 中。 

在外掛程式資料夾內 (在此範例中， ```C:\ProgramData\Server Management Experience\plugins```)

* 使用相同的名稱建立新的資料夾```Name```屬性值```Feature```自訂閘道外掛程式 DLL 中 (在我們的範例專案，```Name```是 「 範例 Uno")
* 將您的自訂閘道外掛程式 DLL 檔案複製到這個新的資料夾
* 重新啟動 Windows Admin Center 程序

Windows 系統管理程序重新啟動之後，您將能夠執行您的自訂閘道外掛程式 DLL 中的 Api 發出 GET，PUT、 PATCH、 DELETE 或張貼到 ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### <a name="optional-attach-to-plugin-for-debugging"></a>選擇性：附加至偵錯的外掛程式

在 Visual Studio 2017 中，從 偵錯 功能表中，選取 附加至處理序 」。 在下一步 視窗中，捲動以查看可用的處理序清單並選取 SMEDesktop.exe，然後按一下 連結。 一次偵錯工具啟動時，您可以將中斷點置於您的功能的程式碼，然後練習透過上述的 URL 格式。 我們的範例專案 (功能名稱：「 範例 Uno") 的 URL 是: 「<http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno>"

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>使用 Windows Admin Center CLI 中建立 [工具] 延伸模組 ##

現在，我們需要建立工具擴充功能，您可以從中呼叫您的自訂閘道外掛程式。  建立或瀏覽至您要儲存專案檔，開啟命令提示字元，並設定該資料夾為工作目錄的資料夾。  使用先前已安裝的 Windows Admin Center CLI，請使用下列語法建立新的延伸模組：

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱 （含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 您的工具名稱 （含空格） | ```Manage Foo Works``` |

這裡提供一個範例用法：

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

這會建立新的資料夾，在目前工作目錄中使用的名稱指定為您的工具，將所有必要範本檔案複製到您的專案，並會設定檔案，以您的公司和工具的名稱。  

接下來，將目錄切換到剛才建立的資料夾，然後執行下列命令來安裝必要的本機相依性：

```
npm install
```

完成之後，您已設定您需要將您新的擴充功能載入 Windows Admin Center 的所有項目。 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>連接您的工具延伸模組加入您的自訂閘道外掛程式

既然您已使用 Windows Admin Center CLI 來建立擴充功能，您已準備好加入您的自訂閘道外掛程式，連接您的工具擴充功能，依照下列步驟：

- 新增[空的模組](guides/add-module.md)
- 使用您[自訂閘道外掛程式](guides/use-custom-gateway-plugin.md)工具延伸模組中
 
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