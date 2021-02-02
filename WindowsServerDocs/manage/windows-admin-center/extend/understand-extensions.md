---
title: 了解 Windows Admin Center 擴充功能
description: 了解 Windows Admin Center SDK 擴充功能 (Project Honolulu)
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 578376a10b83466382bf9e02f91ff94656bd6c88
ms.sourcegitcommit: 84b97d34d606b6bf4b6ec8760a93107f1b311428
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2021
ms.locfileid: "99245475"
---
# <a name="understanding-windows-admin-center-extensions"></a>了解 Windows Admin Center 擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

如果您還不熟悉 Windows Admin Center 的運作方式，讓我們先從高階架構著手。 Windows Admin Center 包含下列兩個主要元件：

- 輕量型 **Web 服務**，依據網頁瀏覽器要求提供 Windows Admin Center UI 網頁。
- **閘道元件**，接聽網頁的 REST API 要求，並轉送要對目標伺服器或叢集執行的 WMI 呼叫或 PowerShell 指令碼。

![Windows Admin Center 架構的圖表。](../media/understand-extensions/wac-architecture-500px.png)

從擴充性觀點來看，Web 服務所提供的 Windows Admin Center UI 網頁有兩個主要 UI 元件，即解決方案與工具 (實作為擴充功能)，以及稱為閘道外掛程式的協力廠商擴充類型。

## <a name="solution-extensions"></a>解決方案擴充功能

在 Windows Admin Center 主畫面中，您預設可以新增其中一種類型的連線，共四種連線類型：Windows Server 連線、Windows 電腦連線、容錯移轉叢集連線和超融合式叢集連線。 新增連線後，連線名稱及類型就會顯示在主畫面中。 按一下連線名稱將會嘗試連線至目標伺服器或叢集，然後載入連線的 UI。

![Windows Admin Center 的 [新增連接] 功能的螢幕擷取畫面。](../media/understand-extensions/solutions-ui.png)

這其中每個連線類型都對應至一個解決方案，而解決方案是透過一種稱為「解決方案」擴充功能的擴充功能類型所定義。 解決方案通常定義您想要透過 Windows Admin Center 管理的獨特物件類型，例如伺服器、電腦或容錯移轉叢集。 您也可以定義新的解決方案，用來連接和管理其他裝置 (例如網路交換器和 Linux 伺服器) 甚或服務 (例如遠端桌面服務)。

## <a name="tool-extensions"></a>工具擴充功能

如果您 Windows Admin Center 主畫面中的連線並進行連線時，將會載入所選連線類型的解決方案擴充功能，然後向您顯示解決方案 UI，包括左瀏覽窗格中的工具清單。 當您按一下工具時，將會載入工具 UI 並顯示在右窗格中。

![Windows Admin Center UI 架構](../media/understand-extensions/ui-architecture.png)

這其中每個工具都是透過第二種稱為「工具」擴充功能的擴充功能類型所定義。 工具載入後，可以對目標伺服器或叢集執行 WMI 呼叫或 PowerShell 指令碼，並在 UI 中顯示資訊，或是根據使用者輸入執行命令。 工具擴充功能定義應針對哪些解決方案顯示該工具，導致每個解決方案都有不同的一組工具。 如果您要建立新的解決方案擴充功能，則需要另外撰寫一個或多個為解決方案提供功能的工具擴充功能。

![每個解決方案的工具清單](../media/understand-extensions/tools-for-solutions.png)

## <a name="gateway-plugins"></a>閘道外掛程式

閘道服務會公開 REST API 供 UI 呼叫並轉送要對目標執行的命令和指令碼。 閘道服務可透過支援各種不同通訊協定的閘道外掛程式來擴充。 Windows Admin Center 已預先封裝兩個閘道外掛程式，一個用於執行 PowerShell 指令碼，另一個用於執行 WMI 命令。 如果您需要透過 PowerShell 或 WMI 以外的通訊協定 (例如 REST) 與目標通訊，您可以為此建置閘道外掛程式。

## <a name="next-steps"></a>下一步

視您要在 Windows Admin Center 建置哪些功能而定，[建置工具擴充功能](develop-tool.md)對現有的伺服器或叢集解決方案來說可能就足夠了，而且是建置擴充功能最簡單的第一步。 不過，如果您的功能是用來管理裝置、服務或某種全新系統，而非伺服器或叢集時，您應該考慮[建置解決方案擴充功能](develop-solution.md)並提供一個或多個工具。 最後，如果您需要透過 WMI 或 PowerShell 以外的通訊協定與目標通訊，您必須 [建立閘道外掛程式](develop-gateway-plugin.md)。 請[繼續閱讀](developing-extensions.md)，了解如何設定開發環境，並開始撰寫您的第一個擴充功能。
