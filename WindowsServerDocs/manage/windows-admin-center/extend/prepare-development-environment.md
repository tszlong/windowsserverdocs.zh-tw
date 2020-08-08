---
title: 準備您的開發環境
description: 準備您的開發環境 Windows Admin Center SDK (Project Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.openlocfilehash: 09d39aa027adf360c339da434b16038a3b8e5c90
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964594"
---
# <a name="prepare-your-development-environment"></a>準備您的開發環境

>適用於：Windows Admin Center、Windows Admin Center 預覽版

讓我們使用 Windows 管理中心 SDK 開始開發擴充功能！  在本檔中，我們將討論讓您的環境啟動並執行的程式，以建立和測試 Windows 系統管理中心的延伸模組。

> [!NOTE]
> 對 Windows Admin Center SDK 不熟悉？  深入了解 [Windows Admin Center 擴充功能](extensibility-overview.md)

若要準備您的開發環境，請執行下列步驟：

## <a name="install-prerequisites"></a>安裝先決條件

若要開始使用 SDK 進行開發，請下載並安裝下列必要條件：

* [Windows 系統管理中心](https://aka.ms/WACDownloadPage) (GA 或預覽版本) 
* Visual Studio 或 [Visual Studio Code](https://code.visualstudio.com)
* [Node.js](https://nodejs.org/en/download/releases/) (版本 10.3.0) 
* [節點套件管理員](https://npmjs.com/get-npm) (8.12.0 或更新版本) 
* [Nuget](https://www.nuget.org/downloads) (用於發佈擴充功能)

> [!NOTE]
> 您必須依照下列步驟，在開發人員模式中安裝並執行 Windows Admin Center。 開發人員模式允許 Windows Admin Center 載入未簽署的擴充功能。 Windows 管理中心只能在 Windows 10 電腦上以 Dev 模式安裝。
>
>  若要啟用開發人員模式，請使用參數 DEV_MODE=1 從命令列中安裝 Windows Admin Center。 在以下範例中，將 ```<version>``` 取代為您要安裝的版本，亦即 ```WindowsAdminCenter1809.msi```。
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>安裝全域相依性

接下來，使用節點套件管理員來安裝或更新專案所需的相依性。 這些相依性將會進行全域安裝，可供所有專案使用。

```
npm install -g npm

npm install -g @angular/cli@7.1.2

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>您可以安裝較新版本的 @angular/cli ，但請注意，如果您安裝的版本大於7.1.2，您會在 gulp 組建步驟期間收到警告，表示本機 cli 版本與已安裝的版本不符。

## <a name="next-steps"></a>後續步驟

既然您的環境已準備就緒，您就可以開始建立內容。

- 建立[工具](develop-tool.md)擴充功能
- 建立[解決方案](develop-solution.md)擴充功能
- 建立[閘道外掛程式](develop-gateway-plugin.md)
- 透過我們的[指南](guides.md)深入了解

## <a name="sdk-design-toolkit"></a>SDK 設計工具組

查看我們的 Windows Admin Center [SDK 設計工具](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)組！ 此工具組的設計目的是要協助您使用 Windows 管理中心樣式、控制項和頁面範本，快速地在 PowerPoint 中模擬延伸模組。 開始撰寫程式碼之前，請先查看您的擴充功能在 Windows 系統管理中心中的樣子。

