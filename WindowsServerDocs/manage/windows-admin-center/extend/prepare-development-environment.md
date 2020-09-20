---
title: 準備您的開發環境
description: 準備您的開發環境 Windows Admin Center SDK (Project Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.openlocfilehash: fe519498e8021bde67b87ec7f78b3e1b9a64160b
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766941"
---
# <a name="prepare-your-development-environment"></a>準備您的開發環境

>適用於：Windows Admin Center、Windows Admin Center 預覽版

現在就開始使用 Windows Admin Center SDK 來開發延伸模組！  在本檔中，我們將討論如何讓您的環境啟動並執行，以建立並測試 Windows Admin Center 的擴充功能。

> [!NOTE]
> 對 Windows Admin Center SDK 不熟悉？  深入了解 [Windows Admin Center 擴充功能](extensibility-overview.md)

若要準備您的開發環境，請執行下列步驟：

## <a name="install-prerequisites"></a>安裝先決條件

若要開始使用 SDK 進行開發，請下載並安裝下列必要條件：

* [Windows Admin Center](../overview.md) (GA 或預覽版本) 
* Visual Studio 或 [Visual Studio Code](https://code.visualstudio.com)
* [Node.js](https://nodejs.org/en/download/releases/) (版本 10.3.0) 
* [節點封裝管理員](https://npmjs.com/get-npm) (8.12.0 或更新版本) 
* [Nuget](https://www.nuget.org/downloads) (用於發佈擴充功能)

> [!NOTE]
> 您必須依照下列步驟，在開發人員模式中安裝並執行 Windows Admin Center。 開發人員模式允許 Windows Admin Center 載入未簽署的擴充功能。 Windows Admin Center 只能安裝在 Windows 10 機上的開發模式。
>
>  若要啟用開發人員模式，請使用參數 DEV_MODE=1 從命令列中安裝 Windows Admin Center。 在以下範例中，將 ```<version>``` 取代為您要安裝的版本，亦即 ```WindowsAdminCenter1809.msi```。
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>安裝全域相依性

接下來，使用 Node 封裝管理員安裝或更新您的專案所需的相依性。 這些相依性將會進行全域安裝，可供所有專案使用。

```
npm install -g npm

npm install -g @angular/cli@7.1.2

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>您可以安裝較新版本的 @angular/cli ，但請注意，如果您安裝的版本大於7.1.2，您會在 gulp 組建步驟期間收到警告，表示本機 cli 版本不符合已安裝的版本。

## <a name="next-steps"></a>後續步驟

現在您的環境已備妥，您已準備好開始建立內容。

- 建立[工具](develop-tool.md)擴充功能
- 建立[解決方案](develop-solution.md)擴充功能
- 建立[閘道外掛程式](develop-gateway-plugin.md)
- 透過我們的[指南](guides.md)深入了解

## <a name="sdk-design-toolkit"></a>SDK 設計工具組

請參閱我們的 Windows Admin Center [SDK 設計工具](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)組！ 此工具組的設計目的是要協助您使用 Windows Admin Center 樣式、控制項和頁面範本，在 PowerPoint 中快速模擬擴充功能。 在開始撰寫程式碼之前，請先看看您的延伸模組看起來的樣子 Windows Admin Center！