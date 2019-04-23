---
title: BranchCache 部署指南
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9bccf69f0a913159a395fabc670a63e2c159bd91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888179"
---
# <a name="branchcache-deployment-guide"></a>BranchCache 部署指南

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本指南以了解如何部署 Windows Server 2016 中的 BranchCache。  
  
除了本主題中，本指南會包含下列各節。  
  
-   [選擇 BranchCache 設計](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [Deploy BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>BranchCache 部署概觀

BranchCache 是某些版本的 Windows Server 2016 中，Windows Server 中內含廣域網路 (wan) 頻寬最佳化技術&reg;2012 R2 中，Windows Server&reg; Windows Server 2012&reg; 2008 R2 和相關Windows 用戶端作業系統。  
  
為了最佳化 WAN 頻寬，BranchCache 會從總公司的內容伺服器複製內容，並在分公司快取內容，讓分公司的用戶端電腦從本機存取內容而非透過 WAN。  
  
在分公司，內容會快取上執行之 BranchCache 功能的 Windows Server 2016 的伺服器、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2-或者，如果有分公司，內容中沒有可用的伺服器是 cac執行 Windows 10 的用戶端電腦上的 hed&reg;，Windows&reg; 8.1、windows 8 或 Windows 7&reg; 。  
  
用戶端電腦要求及接收內容從主要的 office 或雲端資料中心在分公司快取內容後，同一分公司的其他電腦可以取得在本機的內容，而不是透過聯繫的內容伺服器WAN 連結。  
  
**部署 BranchCache 的優點**  
  
BranchCache 快取檔案、 web 和應用程式內容，在分公司，讓用戶端使用區域網路 (LAN) 來存取資料，而不是透過慢速 WAN 連線存取內容的電腦。  
  
BranchCache 可以降低 WAN 流量和所需的分公司辦公室的使用者在網路上開啟檔案的時間。  BranchCache 一律會提供使用者最新的資料，它會用來保護內容的安全性加密託管快取伺服器和用戶端電腦上的快取。  
  
### <a name="what-this-guide-provides"></a>本指南中提供的功能  
此部署指南可讓您以下列模式部署 BranchCache:  
  
-   分散式快取模式。 在此模式中，分公司辦公室用戶端電腦從總公司或雲端中的內容伺服器下載內容，並接著快取中，同一分公司的其他電腦的內容。 分散式快取模式不需要在分公司的伺服器電腦。  
  
-   託管快取模式。 在此模式中，分公司辦公室用戶端電腦下載內容從總公司或雲端中的內容伺服器和託管快取伺服器會從用戶端擷取內容。 託管快取伺服器再快取其他用戶端電腦的內容。  
  
本指南也會提供有關如何部署三種類型的內容伺服器的指示。 內容伺服器包含分公司辦公室用戶端電腦，已下載的來源內容，以及一或多個內容的伺服器，才可在任一種模式中部署 BranchCache。 內容伺服器類型包括：  
  
-   **Web 伺服器為基礎的內容伺服器**。 這些內容伺服器會將內容傳送至使用 HTTP 和 HTTPS 通訊協定的 BranchCache 用戶端電腦。 這些內容伺服器必須執行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 支援 BranchCache 的版本時安裝 BranchCache 功能。  
  
-   **位元為基礎的應用程式伺服器**。 這些內容伺服器會將內容傳送至使用背景智慧型傳送服務 (BITS) 的 BranchCache 用戶端電腦。 這些內容伺服器必須執行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 支援 BranchCache 的版本時安裝 BranchCache 功能。  
  
-   **檔案伺服器為基礎的內容伺服器**。 這些內容伺服器必須執行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 支援 BranchCache 的版本和安裝伺服器角色時的檔案服務。 颾魤 ㄛ**網路檔案的 BranchCache**必須安裝並設定檔案服務伺服器角色的角色服務。 這些內容伺服器會將內容傳送至使用伺服器訊息區塊 (SMB) 通訊協定的 BranchCache 用戶端電腦。  
  
如需詳細資訊，請參閱 < [BranchCache 的作業系統版本](https://technet.microsoft.com/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache)。  
  
### <a name="branchcache-deployment-requirements"></a>BranchCache 的部署需求

以下是使用本指南來部署 BranchCache 的需求。  
  
-   **檔案和 Web 內容伺服器**必須執行下列作業系統提供 BranchCache 功能的其中一個：Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2。 Windows 8 和更新版本的用戶端持續看到來自 BranchCache 的優勢，但無法存取執行 Windows Server 2008 R2，內容伺服器時使用新的區塊化和雜湊在 Windows Server 2016 中，Windows Server 2012 的技術R2 和 Windows Server 2012。  
  
-   **用戶端電腦**必須執行 Windows 10，Windows 8.1 或 Windows 8，將使用的最新的部署模型和區塊處理和雜湊與 Windows Server 2012 引進的增強功能。  
  
-   **託管快取伺服器**必須執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 以使用部署改進並調整這份文件中所述的功能。  執行其中一個已設定為託管快取伺服器這些作業系統的電腦可以繼續執行，服務正在執行 Windows 7 的用戶端電腦，但這樣做，必須配備適用於傳輸層安全性 (TLS 憑證)，在 Windows Server 2008 R2 和 Windows 7 中所述[BranchCache 部署指南](https://technet.microsoft.com/library/ee649232.aspx)。  
  
-   **Active Directory 網域**才能利用群組原則和託管快取自動探索，但若要使用 BranchCache 不需要網域。  您可以使用 Windows PowerShell 來設定個別的電腦。 此外，不需要您的網域控制站都執行 Windows Server 2012 或更新版本，以利用新 BranchCache 群組原則設定;您可以匯入到執行舊版作業系統，網域控制站上的 BranchCache 系統管理範本，或者也可以編寫群組原則物件，在其他執行 Windows 10，Windows Server 2016、 Windows 8.1 的電腦上的遠端Windows Server 2012 R2、 Windows 8 或 Windows Server 2012。

-   **Active Directory 站台**用來限制的託管快取伺服器自動探索的範圍。  若要自動探索託管快取伺服器，用戶端和伺服器電腦必須屬於相同的站台。 BranchCache 的設計具有用戶端和伺服器上的影響最小，並不會造成以外執行其各自的作業系統所需的額外硬體需求。  

**BranchCache 歷程記錄和文件**

Windows 7 中引進 BranchCache&reg;和 Windows Server&reg; 2008 R2，並已改進在 Windows Server 2012 中，Windows 8 和更新版本的作業系統。

> [!NOTE]
> 如果您要在 Windows Server 2016 以外的作業系統部署 BranchCache，以下文件是可用資源。
> 
> - Windows 8、 Windows 8.1、 Windows Server 2012 和 Windows Server 2012 R2 中 BranchCache 的相關資訊，請參閱[BranchCache 概觀](https://technet.microsoft.com/library/hh831696.aspx)。  
> - 在 Windows 7 和 Windows Server 2008 R2 中的 BranchCache 的相關資訊，請參閱[BranchCache for Windows Server 2008 R2](https://technet.microsoft.com/library/dd996634.aspx)。  
  


