---
title: Windows Server 版本 1709 簡介
description: 如何取得、安裝和啟用
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 12/5/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: a2da7b8437a9dc7f44fc83837e3ebad93b7fe3ab
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "1410395"
---
# <a name="introducing-windows-server-version-1709"></a>Windows Server 版本 1709 簡介

>適用於：Windows Server (半年度管道)

**Windows Server 版本 1709 是在新推出的半年度管道中發行的第一個版本。** 

## <a name="what-the-semi-annual-channel-is--and-isnt"></a>半年度管道是什麼，以及不是什麼
Windows Server 版本 1709 是這個新管道中的第一個發行版本，因此*不是* Windows Server 2016 的「更新」或「Service Pack」。 它是**新發行管道**中首個一年兩次伺服器版本，專門設計給追隨「雲端節奏」進展的客戶，例如：處於快速開發週期的客戶，或跟上最新 Hyper-V 投資的主機服務提供者 在此管道中發行的每個版本都會從初始版本開始提供 18 個月的支援。 如需半年度管道的詳細資訊，以及**判斷應加入哪個管道的提示** (或維持不變)，請參閱[半年度管道概觀](semi-annual-channel-overview.md)。


**長期維護管道 (LTSC) 目前的產品是 Windows Server 2016**。 如果您需要伺服器作業系統的長期穩定性與可預測性以便支援傳統工作負載和應用程式，則 LTSC 適合您。 如果您想要繼續留在 LTSC 中，就應該安裝 (或繼續使用) Windows Server 2016，而您可以在 Server Core 模式或「含有桌面體驗的伺服器」模式下進行安裝。 如需詳細資訊，請參閱[開始使用 WindowsServer 2016](https://docs.microsoft.com/windows-server/get-started/server-basics)。


## <a name="whats-different-about-1709"></a>1709 有何不同？

Windows Server 版本 1709 在 Server Core 模式下執行。 這表示沒有圖形化使用者介面，因此您會從遠端進行管理。 不過，卻提供了很大的優勢，例如硬體需求更低、攻擊面大幅縮小，以及需要的更新次數減少。 如果您對使用 Server Core 還很生疏，[管理 Server Core 伺服器](../administration/server-core/server-core-manage.md)可協助您熟悉這個環境。 [管理 Windows Server 2016](../administration/manage-windows-server.md) 會告訴您從遠端管理伺服器的各種不同選項。

[Windows Server 版本 1709 的新功能](whats-new-in-windows-server-1709.md)向您介紹 Windows Server 版本 1709 中加入的新特色及功能。

### <a name="why-does-windows-server-version-1709-offer-only-the-server-core-installation-option"></a>為什麼 Windows Server 版本 1709 只有提供 Server Core 安裝選項？
每一次規劃 Windows Server 發行版本時，我們採用的最重要步驟就是傾聽客戶意見反應：您使用 Windows Server 的情況如何？ 哪些功能會對您的 Windows Server 部署造成極大影響，甚至衝擊日常業務？ 您的意見反應讓我們知道盡快且盡可能有效率提供新創新是主要優先考量。 此時就那些以最快速度進行創新的客戶來看，我們得知他們主要使用 PowerShell 的命令列指令碼來管理資料中心，因此對含有桌面體驗的 Windows Server 安裝中提供的桌面 GUI 沒有強烈需求。 我們只要將重點放在 Server Core 安裝選項，就可以將更多資源投向這些新創新，同時還能維持傳統 Windows Server 平台功能及應用程式相容性。 如果您有關於這一點或其他有關 Windows Server 及未來發行版本之問題的意見反應，可以透過[意見反應中樞](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)提出建議和意見。


### <a name="what-about-nano-server"></a>Nano Server 又會是怎樣呢？
Nano Server 是以容器作業系統形式來提供。 如需詳細資訊，請參閱[Nano Server 在 Windows Server 半年度管道中的變更](nano-in-semi-annual-channel.md)。

## <a name="additional-information-about-this-release"></a>關於本次發行版本的其他資訊
為了全面深入了解 Windows Server 版本 1709 的重要事項，您也應該在進行安裝前，先檢閱下列主題：

- 需要哪些硬體來執行？ 請參閱[系統需求](system-requirements.md)；此版本的系統需求與 Windows Server 2016 相同。
- 新增了哪些新特色及功能？ 請參閱 [Windows Server 版本 1709 的新功能](whats-new-in-windows-server-1709.md)。
- 移除了哪些功能？ 請參閱[從 Windows Server (版本 1709) 開始移除或計劃取代的功能](Removed-Features-1709.md)
- 需要因應解決哪些此版本專屬的問題？ 請參閱[版本資訊：Windows Server 版本 1709 中的重要問題](server-1709-relnotes.md)


## <a name="where-to-obtain-windows-server-version-1709"></a>何處可以取得 Windows Server 版本 1709

此版本應以全新安裝方式進行安裝。

- VLSC：有[軟體保證](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx)的大量授權客戶可以取得此版本，方式為前往[大量授權服務中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)並按一下 **\[登入\]**. 然後按一下 **\[下載和金鑰\]** 並搜尋此版本。 

- Windows Server 版本 1709 也在 [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview) 中提供。

- ** Visual Studio 訂閱**的參與者：如果您已經參加 Visual Studio 訂閱，可以取得 Windows Server 版本 1709，方式為前往 [Visual Studio 訂閱者下載頁面](https://my.visualstudio.com/downloads?pid=2347)並在那裡完成下載。 如果您還不是訂閱者，請前往 [Visual Studio 訂閱](https://www.visualstudio.com/subscriptions/)註冊，然後依上述方式瀏覽 [Visual Studio 訂閱者下載頁面](https://my.visualstudio.com/downloads?pid=2347)。 透過 Visual Studio 訂閱取得的版本僅供開發和測試用途。




## <a name="activating-windows-server-version-1709"></a>啟用 Windows Server 版本 1709

- 如果您已從大量授權服務中心取得此版本，可以使用您的 Windows Server 2016 啟用金鑰進行啟用。
- 如果使用的是 Microsoft Azure，應該會自動啟用此版本。
- 如果是透過 Visual Studio 訂閱取得，也應該會自動啟用此版本。