---
title: BranchCache 部署指南
description: 瞭解如何在 Windows Server 2016 中部署 BranchCache。
manager: brianlic
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 63262b483b4a57b4095a7c99b29b63b2da145247
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904763"
---
# <a name="branchcache-deployment-guide"></a>BranchCache 部署指南

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本指南來瞭解如何在 Windows Server 2016 中部署 BranchCache。

除了本主題之外，本指南還包含下列各節。

-   [選擇 BranchCache 設計](../../branchcache/plan/Choosing-a-BranchCache-Design.md)

-   [部署 BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)

## <a name="branchcache-deployment-overview"></a>BranchCache 部署總覽

BranchCache 是一種廣域網路 (WAN) 頻寬優化技術，包含在某些版本的 Windows Server 2016、Windows Server &reg; 2012 R2、Windows server &reg; 2012、windows Server &reg; 2008 R2 和相關的 windows 用戶端作業系統中。

為了最佳化 WAN 頻寬，BranchCache 會從總公司的內容伺服器複製內容，並在分公司快取內容，讓分公司的用戶端電腦從本機存取內容而非透過 WAN。

在分公司，內容會在執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 之 BranchCache 功能的伺服器上進行快取，或者，如果分公司沒有任何可用的伺服器，就會在執行 Windows 10 &reg; 、Windows &reg; 8.1、Windows 8 或 Windows 7 的用戶端電腦上快取內容 &reg; 。

在用戶端電腦要求並接收總公司或雲端資料中心的內容，並在分公司快取內容之後，同一分公司的其他電腦就可以在本機取得內容，而不是透過 WAN 連結來與內容伺服器聯繫。

**部署 BranchCache 的優點**

BranchCache 會將檔案、web 和應用程式內容快取到分公司位置，讓用戶端電腦可以使用區域網路 (LAN) 來存取資料，而不是透過低速 WAN 連線來存取內容。

BranchCache 可減少 WAN 流量，以及分公司使用者在網路上開啟檔案所需的時間。  BranchCache 一律會為使用者提供最新的資料，並藉由加密託管快取伺服器和用戶端電腦上的快取，來保護內容的安全性。

### <a name="what-this-guide-provides"></a>本指南中提供的功能
此部署指南可讓您以下列模式部署 BranchCache：

-   分散式快取模式。 在此模式中，分公司用戶端電腦會從主要辦公室或雲端中的內容伺服器下載內容，然後快取相同分公司中其他電腦的內容。 分散式快取模式不需要分公司的伺服器電腦。

-   託管快取模式。 在此模式中，分公司用戶端電腦會從主要辦公室或雲端中的內容伺服器下載內容，而託管快取伺服器會從用戶端抓取內容。 然後，託管快取伺服器會快取其他用戶端電腦的內容。

本指南也提供如何部署三種類型的內容伺服器的指示。 內容伺服器包含由分公司用戶端電腦下載的來源內容，而且需要一或多個內容伺服器才能以任一種模式部署 BranchCache。 內容伺服器類型為：

-   以 **Web 服務器為基礎的內容伺服器**。 這些內容伺服器會使用 HTTP 和 HTTPS 通訊協定，將內容傳送至 BranchCache 用戶端電腦。 這些內容伺服器必須執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或支援 BranchCache 的 Windows Server 2008 R2 版本，以及已安裝 BranchCache 功能的 windows server R2 版本。

-   以 **BITS 為基礎的應用程式伺服器**。 這些內容伺服器會使用背景智慧型傳送服務 (位) ，將內容傳送至 BranchCache 用戶端電腦。 這些內容伺服器必須執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或支援 BranchCache 的 Windows Server 2008 R2 版本，以及已安裝 BranchCache 功能的 windows server R2 版本。

-   以 **檔案伺服器為基礎的內容伺服器**。 這些內容伺服器必須執行支援 BranchCache 且已安裝檔案服務伺服器角色的 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 版本。 此外，必須安裝及設定檔案服務伺服器角色的 [網路檔案的 **BranchCache** ] 角色服務。 這些內容伺服器會使用伺服器訊息區，將內容傳送至 BranchCache 用戶端電腦， (SMB) 通訊協定。

如需詳細資訊，請參閱 [BranchCache 的作業系統版本](../branchcache.md#bkmk_os)。

### <a name="branchcache-deployment-requirements"></a>BranchCache 部署需求

以下是使用本指南部署 BranchCache 的需求。

-   檔案 **和 Web 內容伺服器** 必須執行下列其中一個作業系統，以提供 BranchCache 功能： Windows server 2016、windows Server 2012 R2、windows server 2012 或 windows Server 2008 R2。 當您存取執行 Windows Server 2008 R2 的內容伺服器時，Windows 8 和更新版本的用戶端會繼續看到 BranchCache 的優點，但他們無法利用 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的新區塊化與雜湊技術。

-   **用戶端電腦** 必須執行 Windows 10、Windows 8.1 或 Windows 8，才能使用最新的部署模型，以及在 Windows Server 2012 中引進的區塊化與雜湊改進。

-   **託管** 快取伺服器必須執行 windows server 2016、windows Server 2012 R2 或 windows server 2012，才能使用本檔中所述的部署增強功能和調整功能。  如果電腦執行的是下列其中一個設定為託管快取伺服器的作業系統，則可以繼續為執行 Windows 7 的用戶端電腦提供服務，但若要這樣做，它必須配備適用于傳輸層安全性 (TLS) 的憑證，如 Windows Server 2008 R2 和 Windows 7 [BranchCache 部署指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee649232(v=ws.10))中所述。

-   需要 **Active Directory 網域** 才能利用群組原則和託管快取自動探索，但不需要網域就能使用 BranchCache。  您可以使用 Windows PowerShell 來設定個別電腦。 此外，您的網域控制站不需要執行 Windows Server 2012 或更新版本，就能使用新的 BranchCache 群組原則設定;您可以將 BranchCache 系統管理範本匯入到執行舊版作業系統的網域控制站，也可以在其他執行 Windows 10、Windows Server 2016、Windows 8.1、Windows Server 2012 R2、Windows 8 或 Windows Server 2012 的電腦上遠端撰寫群組原則物件。

-   **Active Directory 的網站** 會用來限制自動探索的託管快取伺服器的範圍。  若要自動探索託管快取伺服器，用戶端和伺服器電腦都必須屬於相同的網站。 BranchCache 的設計是對用戶端和伺服器造成極大的影響，而且除了執行其各自的作業系統所需的硬體需求以外，不會強加額外的硬體需求。

**BranchCache 記錄和檔**

BranchCache 首次在 Windows 7 &reg; 和 Windows server &reg; 2008 R2 中引進，並已在 windows Server 2012、Windows 8 和更新版本的作業系統中改善。

> [!NOTE]
> 如果您要在 Windows Server 2016 以外的作業系統中部署 BranchCache，則可以使用下列檔資源。
>
> - 如需 Windows 8、Windows 8.1、Windows Server 2012 和 Windows Server 2012 R2 中 BranchCache 的相關資訊，請參閱 [Branchcache 總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831696(v=ws.11))。
> - 如需有關 Windows 7 和 Windows Server 2008 R2 中 BranchCache 的詳細資訊，請參閱  [Windows server 2008 r2 的 branchcache](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd996634(v=ws.10))。