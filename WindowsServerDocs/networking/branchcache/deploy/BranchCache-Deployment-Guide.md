---
title: BranchCache 部署指南
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 14eb9e5b4d5a28a64d3cfa0d27b5294ba7168da9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356726"
---
# <a name="branchcache-deployment-guide"></a>BranchCache 部署指南

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本指南來瞭解如何在 Windows Server 2016 中部署 BranchCache。  
  
除了本主題之外，本指南還包含下列各節。  
  
-   [選擇 BranchCache 設計](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [部署 BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>BranchCache 部署總覽

BranchCache 是一種廣域網路（WAN）頻寬優化技術，包含在某些版本的 Windows Server 2016、Windows Server&reg; 2012 R2、Windows Server&reg; 2012、Windows Server&reg; 2008 R2 和相關的 Windows 用戶端作業系統中。  
  
為了最佳化 WAN 頻寬，BranchCache 會從總公司的內容伺服器複製內容，並在分公司快取內容，讓分公司的用戶端電腦從本機存取內容而非透過 WAN。  
  
在分公司，內容會在執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的 BranchCache 功能的伺服器上進行快取; 或者，如果分公司沒有可用的伺服器，則會在執行 Windows 10&reg;、Windows&reg; 8.1、Windows 8 或 Windows 7&reg; 的用戶端電腦上快取內容。  
  
在用戶端電腦要求並接收來自總公司或雲端資料中心的內容，並在分公司快取內容之後，相同分公司的其他電腦就可以在本機取得內容，而不是透過來聯繫內容伺服器。WAN 連結。  
  
**部署 BranchCache 的優點**  
  
BranchCache 會在分公司位置快取檔案、web 和應用程式內容，讓用戶端電腦可以使用區域網路（LAN）存取資料，而不是透過慢速 WAN 連線存取內容。  
  
BranchCache 可減少 WAN 流量，以及分公司使用者在網路上開啟檔案所需的時間。  BranchCache 一律會為使用者提供最新的資料，並藉由加密託管快取伺服器和用戶端電腦上的快取，來保護內容的安全性。  
  
### <a name="what-this-guide-provides"></a>本指南中提供的功能  
此部署指南可讓您以下列模式部署 BranchCache：  
  
-   分散式快取模式。 在此模式中，分公司用戶端電腦會從總公司或雲端中的內容伺服器下載內容，然後快取相同分公司中其他電腦的內容。 分散式快取模式不需要分公司的伺服器電腦。  
  
-   託管快取模式。 在此模式中，分公司用戶端電腦會從總公司或雲端中的內容伺服器下載內容，而託管快取伺服器會從用戶端抓取內容。 然後，託管快取伺服器會快取其他用戶端電腦的內容。  
  
本指南也提供如何部署三種內容伺服器類型的指示。 內容伺服器包含分公司用戶端電腦所下載的來源內容，而且需要一或多個內容伺服器，才能在任一種模式中部署 BranchCache。 內容伺服器類型如下：  
  
-   以**網頁伺服器為基礎的內容伺服器**。 這些內容伺服器會使用 HTTP 和 HTTPS 通訊協定，將內容傳送至 BranchCache 用戶端電腦。 這些內容伺服器必須執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或支援 BranchCache 的 Windows Server 2008 R2 版本，並在其上安裝 BranchCache 功能。  
  
-   以**位為基礎的應用程式伺服器**。 這些內容伺服器會使用背景智慧型傳送服務（BITS）將內容傳送至 BranchCache 用戶端電腦。 這些內容伺服器必須執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或支援 BranchCache 的 Windows Server 2008 R2 版本，並在其上安裝 BranchCache 功能。  
  
-   以**檔案伺服器為基礎的內容伺服器**。 這些內容伺服器必須執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或支援 BranchCache 且已安裝檔案服務伺服器角色的 windows Server 2008 R2 版本。 此外，必須安裝和設定檔案服務伺服器角色的 [網路檔案的**BranchCache** ] 角色服務。 這些內容伺服器會使用伺服器訊息區（SMB）通訊協定，將內容傳送至 BranchCache 用戶端電腦。  
  
如需詳細資訊，請參閱[BranchCache 的作業系統版本](https://technet.microsoft.com/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache)。  
  
### <a name="branchcache-deployment-requirements"></a>BranchCache 部署需求

以下是使用本指南來部署 BranchCache 的需求。  
  
-   檔案**和 Web 內容伺服器**必須執行下列其中一種作業系統，才能提供 BranchCache 功能： windows server 2016、windows Server 2012 R2、windows server 2012 或 windows Server 2008 R2。 Windows 8 和更新版本的用戶端在存取執行 Windows Server 2008 R2 的內容伺服器時，會繼續看到 BranchCache 的優點，但是無法利用 Windows Server 2016、Windows Server 2012 中的新區塊化和雜湊技術R2 和 Windows Server 2012。  
  
-   **用戶端電腦**必須執行 windows 10、Windows 8.1 或 Windows 8，才能使用最新的部署模型，以及 windows Server 2012 引進的區塊化和雜湊改良功能。  
  
-   **託管**快取伺服器必須執行 Windows server 2016、windows Server 2012 R2 或 Windows server 2012，才能使用本檔中所述的部署改進和擴充功能。  執行設定為託管快取伺服器的其中一個作業系統的電腦，可以繼續為執行 Windows 7 的用戶端電腦提供服務，但若要這樣做，則必須配備適用于傳輸層安全性（TLS）的憑證，如 Windows Server 2008 R2 和 Windows 7 [BranchCache 部署指南](https://technet.microsoft.com/library/ee649232.aspx)中所述。  
  
-   需要**Active Directory 網域**，才能利用群組原則和託管快取自動探索，但不需要網域就能使用 BranchCache。  您可以使用 Windows PowerShell 來設定個別電腦。 此外，您的網域控制站不需要執行 Windows Server 2012 或更新版本，就能使用新的 BranchCache 群組原則設定;您可以將 BranchCache 系統管理範本匯入到執行舊版作業系統的網域控制站，也可以在其他執行 Windows 10、Windows Server 2016、Windows 8.1 的電腦上遠端撰寫群組原則物件。Windows Server 2012 R2、Windows 8 或 Windows Server 2012。

-   **Active Directory 網站**是用來限制自動探索之託管快取伺服器的範圍。  若要自動探索託管快取伺服器，用戶端和伺服器電腦都必須屬於相同的網站。 BranchCache 的設計是對用戶端和伺服器產生最小的影響，而且不會強加執行其個別作業系統所需的額外硬體需求。  

**BranchCache 歷程記錄和檔**

BranchCache 最初是在 Windows 7&reg; 和 Windows Server&reg; 2008 R2 中引進，並已在 Windows Server 2012、Windows 8 和更新版本的作業系統中改進。

> [!NOTE]
> 如果您要在 Windows Server 2016 以外的作業系統中部署 BranchCache，可以使用下列檔資源。
> 
> - 如需 Windows 8、Windows 8.1、Windows Server 2012 和 Windows Server 2012 R2 中 BranchCache 的相關資訊，請參閱[Branchcache 總覽](https://technet.microsoft.com/library/hh831696.aspx)。  
> - 如需有關 Windows 7 和 Windows Server 2008 R2 中 BranchCache 的詳細資訊，請參閱[Windows Server 2008 r2 的 branchcache](https://technet.microsoft.com/library/dd996634.aspx)。  
  


