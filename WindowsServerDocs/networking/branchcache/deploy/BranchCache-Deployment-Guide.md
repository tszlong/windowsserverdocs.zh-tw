---
title: BranchCache 部署指南
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7aa35260213a8a236b7c27cf74e36692438cd2a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-deployment-guide"></a>BranchCache 部署指南

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此快速入門，以了解如何部署 Windows Server 2016 中的 BranchCache。  
  
本主題中，除了本指南包含下列各節。  
  
-   [選擇 BranchCache 設計](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [部署 BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>BranchCache 部署概觀

BranchCache 是隨附於某些版本的 Windows Server 2016、 Windows Server 寬區域 (wan) 頻寬最佳化技術&reg;2012 R2、 Windows Server&reg; 2012、 Windows Server&reg; 2008 R2 和相關的 Windows client 作業系統。  
  
若要最佳化 WAN 的頻寬，BranchCache 複製 content 從您的主要辦公室內容伺服器，並快取 content 分公司的位置，以允許 client 分公司本機而不是透過 WAN 存取 content 在的電腦。  
  
在分公司 content 快取上執行 Windows Server 2016 的 BranchCache 功能伺服器、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2-，或分公司中有不伺服器提供，如果快取 client 的電腦執行 Windows 10 上的 content&reg;，Windows&reg; 8.1、 Windows 8 或 Windows 7&reg; 。  
  
Client 電腦要求 content 接收主要辦公室或雲端 datacenter 及 content 分公司快取之後，相同分公司其他電腦就可以取得本機物件，而非連絡內容伺服器 WAN 連結。  
  
**部署 BranchCache 的優點**  
  
BranchCache 快取的檔案、 網頁和應用程式 content 分公司的位置，以允許 client 存取資料的使用的區域網路 （區域網路） 而不是存取 content 保守型 WAN 連接到電腦。  
  
BranchCache 降低 WAN 流量和分公司使用者打開網路上的檔案所需的時間。  BranchCache 一律可讓使用者最新的資料，並保護您的安全性，以加密裝載快取伺服器上及 client 電腦上的快取。  
  
### <a name="what-this-guide-provides"></a>本指南所提供的項目  
部署本文可讓您下列模式在組織中部署 BranchCache︰  
  
-   快取分散式的模式。 在此模式下，從中主要辦公室或雲端，內容伺服器下載 content 分支 office client 電腦，然後快取的其他電腦，在同一個分公司 content。 快取分散式的模式不需要在分公司伺服器電腦。  
  
-   裝載快取模式。 在此模式下，分支 office client 電腦下載內容伺服器中主要辦公室或雲端，和裝載快取伺服器，請從擷取 content。 然後，裝載快取伺服器快取 content 其他 client 電腦。  
  
本指南也會提供如何部署三種類型的內容伺服器上的指示。 內容伺服器包含分支 office client 電腦的下載來源 content 而一或多個內容伺服器部署 BranchCache 模式。 內容伺服器類型︰  
  
-   **網頁伺服器端內容伺服器]**。 這些內容的伺服器來傳送 content BranchCache client 電腦使用的 HTTP 和 HTTPS 通訊協定。 這些內容伺服器必須執行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 的支援 BranchCache 版本和時已安裝的 BranchCache 功能。  
  
-   **位元為基礎的應用程式伺服器]**。 這些內容的伺服器來傳送 content BranchCache client 電腦使用背景聰明傳輸服務 （位元）。 這些內容伺服器必須執行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 的支援 BranchCache 版本和時已安裝的 BranchCache 功能。  
  
-   **檔案伺服器端內容伺服器]**。 這些內容伺服器必須執行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2 的支援 BranchCache 版本並安裝時，檔案服務伺服器角色。 此外， **BranchCache 網路檔案的**必須安裝和設定檔服務伺服器角色服務。 這些內容的伺服器來傳送 content BranchCache client 電腦使用伺服器訊息區 (SMB) 通訊協定。  
  
如需詳細資訊，請查看[適用於 BranchCache 作業系統版本](https://technet.microsoft.com/en-us/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache)。  
  
### <a name="branchcache-deployment-requirements"></a>BranchCache 部署需求

以下是使用本指南部署 BranchCache 的需求。  
  
-   **檔案和 Web 內容伺服器]**必須執行其中一個提供 BranchCache 功能下列作業系統： Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2。 Windows 8 和稍後戶端繼續查看受惠於 BranchCache 不過，他們就能無法存取內容伺服器正在執行 Windows Server 2008 R2 的新區塊 hashing 技術 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 中的使用。  
  
-   **Client 電腦**必須執行 Windows 10、 Windows 8.1 或 Windows 8 使用最新的部署模型區塊和 hashing 引進了 Windows Server 2012 的改進。  
  
-   **裝載快取伺服器]**必須執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 使用 deployment 改良功能和縮放本文件中所述的功能。  電腦執行其中一種作業系統的設定視做 client 電腦正在執行 Windows 7 中，但若要這樣做，可以繼續裝載快取伺服器、 必須配備適用於傳輸層安全性 (TLS)、 Windows 7 與 Windows Server 2008 R2 中所述的憑證[BranchCache 部署指南](https://technet.microsoft.com/en-us/library/ee649232.aspx)。  
  
-   **Active Directory domain**才能利用的群組原則和自動尋找裝載快取，但不是一定要使用 BranchCache 網域。  您可以使用 Windows PowerShell 來設定個人電腦。 此外，不需要您的網域控制站執行 Windows Server 2012 或更新版本，才能使用新的 BranchCache 群組原則設定。您可以匯入 BranchCache 系統管理範本加入網域控制站執行更早版本作業系統，或您可以製作群組原則物件遠端在其他電腦正在執行 Windows 10、 Windows Server 2016、 Windows 8.1、 Windows Server 2012 R2、 Windows 8 或 Windows Server 2012 上。

-   **Active Directory 網站**用於限制裝載快取伺服器上的自動發現的範圍。  自動尋找裝載快取伺服器的 client 和伺服器的電腦必須是相同的網站。 BranchCache 的設計目的是戶端伺服器上影響降到最低，並不會對以外執行各自作業系統需要額外的硬體需求。  

**歷史 BranchCache 和文件**

第一次 BranchCache 導入了在 Windows 7&reg;和 Windows Server&reg; 2008 R2 和已改進在 Windows Server 2012、 Windows 8 和較新的作業系統。

> [!NOTE]
> 如果您要部署 BranchCache 作業系統中 Windows Server 2016 以外，下列文件資源可用的。
> 
> - 適用於 Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2 中 BranchCache 的相關資訊，請查看[BranchCache 概觀](https://technet.microsoft.com/en-us/library/hh831696.aspx)。  
> - 適用於 Windows 7 和 Windows Server 2008 R2 BranchCache 的相關資訊，請查看[BranchCache 適用於 Windows Server 2008 R2](https://technet.microsoft.com/en-us/library/dd996634.aspx)。  
  


