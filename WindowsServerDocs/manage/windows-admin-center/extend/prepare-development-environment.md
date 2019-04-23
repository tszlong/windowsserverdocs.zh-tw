---
title: 準備您的開發環境
description: 準備您的開發環境 Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b1a0672ee374f3e2d1339c43576db0e5cabdc36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834759"
---
# <a name="prepare-your-development-environment"></a>準備您的開發環境

>適用於：Windows Admin Center，Windows Admin Center 預覽

我們來開始使用 Windows Admin Center SDK 開發擴充功能！  在本文件中，我們將討論以取得您的環境設定和執行建置和測試 Windows Admin Center 的擴充功能的程序。

> [!NOTE]
> 對 Windows Admin Center SDK 不熟悉？  深入了解 [Windows Admin Center 擴充功能](extensibility-overview.md)

若要準備您的開發環境，請執行下列步驟：

## <a name="install-prerequisites"></a>安裝必要條件

若要開始使用 SDK 進行開發，請下載並安裝下列必要條件：

* [Windows Admin Center](https://aka.ms/WACDownloadPage) （GA 或預覽版的版本）
* Visual Studio 或 [Visual Studio Code](http://code.visualstudio.com)
* [Node 封裝管理員](https://npmjs.com/get-npm)(8.12.0 或更新版本)
* [Nuget](https://www.nuget.org/downloads) (用於發佈擴充功能)

> [!NOTE]
> 您必須依照下列步驟，在開發人員模式中安裝並執行 Windows Admin Center。 開發人員模式允許 Windows Admin Center 載入未簽署的擴充功能。
>
>  若要啟用開發人員模式，請使用參數 DEV_MODE=1 從命令列中安裝 Windows Admin Center。 在以下範例中，將 ```<version>``` 取代為您要安裝的版本，亦即 ```WindowsAdminCenter1809.msi```。
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>安裝通用的相依性

接下來，安裝或更新您的專案，使用節點封裝管理員所需的相依性。 這些相依性將會進行全域安裝，可供所有專案使用。

```
npm install -g npm

npm install -g @angular/cli@1.6.5

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>您可以安裝較新版@angular/cli，不過請注意，如果您安裝的版本大於 1.6.5，您會收到警告 gulp 組建步驟期間的本機的 cli 版本不符合安裝的版本。

## <a name="next-steps"></a>後續步驟

現在，您的環境已準備就緒，您就開始建立內容。

- 建立[工具](develop-tool.md)擴充功能
- 建立[解決方案](develop-solution.md)擴充功能
- 建立[閘道外掛程式](develop-gateway-plugin.md)
- 透過我們的[指南](guides.md)深入了解

## <a name="sdk-design-toolkit"></a>SDK 的設計工具組

請參閱我們的 Windows Admin Center [SDK 的設計工具組](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)！ 此工具組可協助您快速地模擬在 PowerPoint 中使用 Windows Admin Center 樣式、 控制項和頁面範本的延伸模組。 請參閱您的延伸模組可以如下所示的 Windows Admin Center 之前您開始撰寫程式碼 ！

