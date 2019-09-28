---
title: 開發閘道外掛程式
description: 開發閘道外掛程式 Windows 系統管理中心 SDK （Project 檀香山）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 2b096b226190ad1ca3fd07c38b7b939d019ee30f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406935"
---
# <a name="develop-a-gateway-plugin"></a>開發閘道外掛程式

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows 管理中心閘道外掛程式可讓您從工具或解決方案的 UI，對目標節點進行 API 通訊。  Windows 管理中心會裝載閘道服務，以從閘道外掛程式轉送要在目標節點上執行的命令和腳本。 閘道服務可以擴充以包含支援預設通訊協定以外的自訂閘道外掛程式。

根據預設，這些閘道外掛程式會包含在 Windows 管理中心內：

* PowerShell 閘道外掛程式
* WMI 閘道外掛程式

如果您想要與 PowerShell 或 WMI 以外的通訊協定進行通訊（例如使用 REST），您可以建立自己的閘道外掛程式。  閘道外掛程式會從現有的閘道進程載入至不同的 AppDomain，但對許可權使用相同層級的提升。

> [!NOTE]
> 不熟悉不同的延伸模組類型嗎？ 深入瞭解延伸模組[架構和擴充功能類型](understand-extensions.md)。

## <a name="prepare-your-environment"></a>準備您的環境

如果您還沒有這麼做，請安裝所有專案所需的相依性和全域必要條件來[準備您的環境](prepare-development-environment.md)。

## <a name="create-a-gateway-plugin-c-library"></a>建立閘道外掛程式（C#程式庫）

若要建立自訂閘道外掛程式，請C#建立新的類別，以從 ```Microsoft.ManagementExperience.FeatureInterfaces``` 命名空間中執行 @no__t 1 介面。  

> [!NOTE]
> 舊版 SDK 中提供的 @no__t 0 介面現在標示為過時。  所有閘道外掛程式開發應該使用 IPlugIn （或選擇性的 HttpPlugIn 抽象類別）。

### <a name="download-sample-from-github"></a>從 GitHub 下載範例

若要快速開始使用自訂閘道外掛程式，您可以從我們的 Windows 管理中心 SDK [GitHub 網站](https://aka.ms/wacsdk)複製或下載[ C#範例外掛程式專案](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)的複本。

### <a name="add-content"></a>新增內容

將新內容新增至您的[ C#範例外掛程式專案](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin)專案（或您自己的專案）複製的複本，以包含您的自訂 api，然後建立自訂閘道外掛程式 DLL 檔案，以用於後續步驟。

### <a name="deploy-plugin-for-testing"></a>部署外掛程式以進行測試

藉由將您的自訂閘道外掛程式 DLL 載入 Windows 管理中心閘道程式來進行測試。

Windows 系統管理中心會在目前電腦的 [應用程式資料] 資料夾中，尋找 @no__t 0 資料夾中的所有外掛程式（使用 SpecialFolder 列舉的 CommonApplicationData 值）。 在 Windows 10 上，此位置為 ```C:\ProgramData\Server Management Experience```。  如果 @no__t 0 資料夾尚未存在，您可以自行建立資料夾。

> [!NOTE]
> 您可以藉由更新 "StaticsFolder" 設定值，覆寫偵錯工具組建中的外掛程式位置。 如果您是在本機進行偵錯工具，則此設定位於桌面解決方案的 app.config 中。 

在 [外掛程式] 資料夾中（在此範例中，```C:\ProgramData\Server Management Experience\plugins```）

* 建立新資料夾，其名稱與您自訂閘道外掛程式 DLL 中 ```Feature``` 的 ```Name``` 屬性值相同（在我們的範例專案中，```Name``` 為 "Sample Uno"）
* 將您的自訂閘道外掛程式 DLL 檔案複製到這個新資料夾
* 重新開機 Windows 系統管理中心進程

Windows 系統管理員程式重新開機之後，您將能夠在您的自訂閘道外掛程式 DLL 中執行 Api，方法是發出 GET、PUT、PATCH、DELETE 或 POST 給 ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### <a name="optional-attach-to-plugin-for-debugging"></a>選擇性：附加至外掛程式以進行偵錯工具

在 Visual Studio 2017 的 [偵錯工具] 功能表中，選取 [附加至進程]。 在下一個視窗中，流覽 [可使用的進程] 清單，選取 [SMEDesktop]，然後按一下 [附加]。 偵錯工具啟動之後，您可以將中斷點放在您的功能程式碼中，然後透過上述的 URL 格式進行練習。 針對我們的範例專案（功能名稱："Sample Uno"） URL 為： "<http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno>"

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>使用 Windows Admin Center CLI 建立工具擴充功能 ##

現在，我們需要建立可供您呼叫自訂閘道外掛程式的工具擴充功能。  建立或流覽至您要儲存專案檔案的資料夾，開啟命令提示字元，並將該資料夾設定為工作目錄。  使用稍早安裝的 Windows Admin Center CLI，以下列語法建立新的擴充功能：

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱（含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 您的工具名稱（含空格） | ```Manage Foo Works``` |

這裡提供一個範例用法：

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

這會使用您為工具指定的名稱，在目前的工作目錄內建立新的資料夾，將所有必要的範本檔案複製到您的專案中，並使用您的公司和工具名稱來設定檔案。  

接下來，將目錄切換到剛才建立的資料夾，然後執行下列命令來安裝必要的本機相依性：

```
npm install
```

完成後，您就可以設定將新延伸模組載入 Windows 管理中心所需的所有專案。 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>將您的工具擴充功能連接到您的自訂閘道外掛程式

既然您已使用 Windows 管理中心 CLI 建立擴充功能，您可以遵循下列步驟，將您的工具擴充功能連接到您的自訂閘道外掛程式：

- 新增[空白模組](guides/add-module.md)
- 在您的工具擴充功能中使用您的[自訂閘道外掛程式](guides/use-custom-gateway-plugin.md)
 
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