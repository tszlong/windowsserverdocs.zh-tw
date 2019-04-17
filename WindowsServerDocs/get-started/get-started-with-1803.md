---
title: Windows Server 版本 1803 簡介
description: 如何取得、安裝和啟用
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 05/02/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: c5cd8fbcf8424fa158ad31ca64e3eabe426240a6
ms.sourcegitcommit: 8e2903c9b58646840eedd63b47a9bba6c6a06bf7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2018
ms.locfileid: "1859869"
---
# <a name="introducing-windows-server-version-1803"></a>Windows Server 版本 1803 簡介

>適用於：Windows Server (半年通道)

**Windows Server 版本 1803 是 current release in 新推出半年通道中目前的版本**


## <a name="what-the-semi-annual-channel-is--and-isnt"></a>半年通道是什麼，以及不是什麼
Windows Server 版本 1803 *不是* Windows Server 2016 的「更新」或「Service Pack」。 這是發行管道中目前的一年兩次伺服器版本，專門設計給依「雲端發佈頻率」進展的客戶，例如那些處於快速開發週期的客戶。 這個管道非常適合現代應用程式和創新案例，例如容器和微型服務。 此管道中發行的每個版本都會從初始版本開始提供 18 個月的支援。 如需半年通道的詳細資訊，以及**判斷應加入哪個通道的提示** (或維持不變)，請參閱[半年通道概觀](semi-annual-channel-overview.md)。


**Windows Server 2016 是目前的長期維護通道 (LTSC) 產品**。 如果您需要伺服器作業系統的長期穩定性與可預測性以便支援傳統工作負載和應用程式，則 LTSC 很適合您。 如果您想要繼續留在 LTSC 中，就應該安裝 (或繼續使用) Windows Server 2016，而您可以在 Server Core 模式或「含有桌面體驗的伺服器」模式下進行安裝。 如需詳細資訊，請參閱[開始使用 WindowsServer 2016](https://docs.microsoft.com/windows-server/get-started/server-basics)。


## <a name="whats-different-about-windows-server-version-1803"></a>Windows Server 版本 1803 有何不同？

Windows Server 版本 1803 在 Server Core 模式下執行。 Windows Server Core 模式很大的優勢，例如硬體需求更低、攻擊面大幅縮小，以及需要的更新次數減少。 由於沒有使用者介面，Windows Server Core 模式最適合從遠端進行管理。 如果您對使用 Server Core 還很生疏，[管理 Server Core 伺服器](../administration/server-core/server-core-manage.md)可協助您熟悉這個環境。 [管理 Windows Server 2016](../administration/manage-windows-server.md) 會告訴您從遠端管理伺服器的各種不同選項。

[Windows Server 版本 1803 的新功能](whats-new-in-windows-server-1803.md)向您介紹 Windows Server 版本 1803 中加入的新特色及功能。

### <a name="why-does-windows-server-version-1803-offer-only-the-server-core-installation-option"></a>為什麼 Windows Server 版本 1803 只有提供 Server Core 安裝選項？
每一次規劃 Windows Server 發行版本時，我們採用的最重要步驟就是傾聽客戶意見反應：您使用 Windows Server 的情況如何？ 哪些功能會對您的 Windows Server 部署造成極大影響，甚至衝擊日常業務？ 您的意見反應讓我們知道盡快且盡可能有效率提供新創新是主要優先考量。 此時就那些以最快速度進行創新的客戶來看，我們得知他們主要使用 PowerShell 的命令列指令碼來管理資料中心，因此對含有桌面體驗的 Windows Server 安裝中提供的桌面 GUI 沒有強烈需求。 我們只要將重點放在 Server Core 安裝選項，就可以將更多資源投向這些新創新，同時還能維持傳統 Windows Server 平台功能及應用程式相容性。 如果您有關於這一點或其他有關 Windows Server 及未來發行版本之問題的意見反應，可以透過[意見反應中樞](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)提出建議和意見。


### <a name="what-about-nano-server"></a>Nano Server 又會是怎樣呢？
Nano Server 是以容器作業系統形式來提供。 如需詳細資訊，請參閱[Nano Server 在 Windows Server 半年度管道中的變更](nano-in-semi-annual-channel.md)。

## <a name="additional-information-about-this-release"></a>關於本次發行版本的其他資訊
為了全面深入了解 Windows Server 版本 1803 的重要事項，您也應該在進行安裝前，先檢閱下列主題：

- 需要哪些硬體來執行？ 請參閱[系統需求](system-requirements.md)；此版本的系統需求與 Windows Server 2016 相同。
- 新增了哪些新特色及功能？ 請參閱 [Windows Server 版本 1803 的新功能](whats-new-in-windows-server-1803.md)。
- 移除了哪些功能？ 請參閱[從 Windows Server 版本 1803 開始移除或計劃取代的功能](windows-server-1803-removed-features.md)
- 需要因應解決哪些專屬於此版本的問題？ 請參閱[版本資訊：Windows Server 版本 1803 中的重要問題](server-1803-release-notes.md)


## <a name="where-to-obtain-windows-server-version-1803"></a>何處可以取得 Windows Server 版本 1803

此版本應以全新安裝方式進行安裝。

- 大量授權服務中心 (VLSC)：有[軟體保證](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx)的大量授權客戶可以取得此版本，[大量授權服務中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)並按一下 **\[登入\]**。 然後按一下 **\[下載和金鑰\]** 並搜尋此版本。 

- Windows Server 版本 1803 也有在 [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview) 中提供。

- Visual Studio 訂閱：Visual Studio 訂閱者可以從 [Visual Studio 訂閱者下載頁面](https://my.visualstudio.com/downloads?pid=2347)取得 Windows Server 版本 1803。 如果您還不是訂閱者，請前往 [Visual Studio 訂閱](https://www.visualstudio.com/subscriptions/)註冊，然後依上述方式瀏覽 [Visual Studio 訂閱者下載頁面](https://my.visualstudio.com/downloads?pid=2347)。 透過 Visual Studio 訂閱取得的版本僅供開發和測試用途。




## <a name="activating-windows-server-version-1803"></a>啟用 Windows Server 版本 1803

- 如果您已從大量授權服務中心取得此版本，可以透過金鑰管理系統 (KMS) 環境使用您的 Windows Server 2016 CSVLK 進行啟用。
- 如果使用的是 Microsoft Azure，應該會自動啟用此版本。
- 如果您是從 Visual Studio 訂閱取得此版本，則可以透過金鑰管理系統 (KMS) 環境使用您的 Windows Server 2016 CSVLK 進行啟用。 