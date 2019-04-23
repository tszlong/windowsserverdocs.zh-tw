---
title: 部署 BranchCache 託管快取模式
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 電腦上的託管快取模式部署 BranchCache 的指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc2cb29f0f00c04c4208bd83d70bc4d966bbad00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839349"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>部署 BranchCache 託管快取模式

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Windows Server 2016 核心網路指南提供規劃和部署全功能網路與新的 Active Directory 所需的核心元件的指示&reg;新樹系中的網域。

本指南說明如何建置核心網路上，藉由提供具有 「 讀取一或多個分公司的託管快取模式部署 BranchCache 的指示\-只有用戶端電腦執行 Windows 的所在的網域控制站&reg;10，Windows 8.1 或 Windows 8 並已加入網域。

>[!IMPORTANT]
>如果您打算部署或已部署執行 Windows Server 2008 R2 的 BranchCache 託管快取伺服器，請不要使用本指南。 本指南提供指示來部署託管快取模式與託管快取伺服器是執行 Windows Server&reg; 2016，Windows Server 2012 R2 或 Windows Server 2012。

本指南涵蓋下列各節。

- [使用本指南的必要條件](#bkmk_pre)

- [關於本指南](#bkmk_about)

- [本指南中未提供的內容](#bkmk_not)

- [技術概觀](#bkmk_tech)

- [BranchCache 託管快取模式部署概觀](2-Bc-Hcm-Deploy-Overview.md)

- [BranchCache 託管快取模式部署規劃](3-Bc-Hcm-Plan.md)

- [BranchCache 託管快取模式部署](4-Bc-Hcm-Deployment.md)

- [其他資源](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>使用本指南的必要條件

這是 Windows Server 2016 核心網路指南的附屬指南。 若要使用此指南部署託管快取模式的 BranchCache，您必須先執行以下作業：

- 使用《核心網路指南》在總公司部署核心網路，或是在您的網路上已經安裝《核心網路指南》中提供的技術且正常運作。 這些技術包括 TCP\/IP v4，DHCP，Active Directory 網域服務\(AD DS\)，和 DNS。

    > [!NOTE]
    > Windows Server 2016[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)位於 Windows Server 2016 技術文件庫。  

- 部署 BranchCache 內容伺服器執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 在總公司或雲端資料中心內。 如需如何部署 BranchCache 內容伺服器的詳細資訊，請參閱[額外的資源](11-Bc-Hcm-additional-resources.md)。

- 建立廣域網路\(WAN\)分公司，總公司之間的連線並在適當時，您的雲端資源，使用虛擬私人網路\(VPN\)，DirectAccess 或其他連線方式。

- 部署在分公司的用戶端電腦執行下列作業系統，提供支援背景智慧型傳送服務 (BITS) 」、 「 超文字傳輸通訊協定 (HTTP) 和 「 伺服器訊息區塊 (SMB) 中的 BranchCache 的其中一個.
    - Windows 10 企業版
    - Windows 10 教育版
    - Windows 8.1 Enterprise
    - Windows 8 企業版

>[!NOTE]
>在下列作業系統中，BranchCache 不支援 HTTP 和 SMB 功能，但可支援 BranchCache 的位元的功能。
>     - Windows 10 專業版，位元僅支援
>     - Windows 8.1 專業版、 位元僅支援
>     - Windows 8 專業版，位元僅支援

## <a name="bkmk_about"></a>關於本指南

本指南旨在針對網路和系統管理員已經在 Windows Server 2016 核心網路指南或 Windows Server 2012 核心網路指南 》 中的指示部署核心網路，或先前已部署的任何人包含核心網路指南，包括 Active Directory 網域服務技術\(AD DS\)，網域名稱服務\(DNS\)，動態主機設定通訊協定\(DHCP\)，和 TCP\/IP v4。

建議您檢閱此部署案例中所用每個技術的設計和部署指南。 這些指南可協助您判斷此部署案例是否提供貴組織的網路需要的服務與設定。

## <a name="bkmk_not"></a>本指南未提供的內容

本指南未提供 BranchCache 的概念資訊，包括關於 BranchCache 模式和功能的資訊。  

本指南未提供如何在分公司部署 WAN 連線或其他技術 (如 DHCP、RODC 或 VPN 伺服器) 的資訊。

此外，本指南未提供部署託管快取伺服器應使用之硬體的相關指引。 您可以在託管快取伺服器上執行其他服務和應用程式，但是您必須根據工作負載、硬體功能、分公司規模大小等因素進行判斷，是否要在特定電腦上安裝 BranchCache 託管快取伺服器，以及要為快取配置多少磁碟空間。  
本指南不提供執行 Windows 7 的電腦的設定指示。 如果您有分公司中執行 Windows 7 的用戶端電腦時，您必須設定它們使用不同於本指南中提供執行 Windows 10、 Windows 8.1 及 Windows 8 的用戶端電腦的程序。
  
此外，如果您有執行 Windows 7 的電腦，您必須設定您的託管快取伺服器由用戶端電腦信任的憑證授權單位發行伺服器憑證。 \(如果所有用戶端電腦都執行 Windows 10，Windows 8.1 或 Windows 8，則您不需要使用伺服器的憑證設定託管快取伺服器。\) 
> [!IMPORTANT]
> 如果您的託管快取伺服器執行 Windows Server 2008 R2，請使用 Windows Server 2008 R2 [BranchCache 部署指南](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx)而不是本指南來部署 BranchCache 託管快取模式。 套用至所有正在執行從 Windows 7 的 Windows 版本為 Windows 10 的 BranchCache 用戶端該指南中所述的群組原則設定。 無法使用本指南中的步驟，設定執行 Windows Server 2008 R2 的電腦。

## <a name="bkmk_tech"></a>技術概觀

本附屬指南中，BranchCache 是您唯一需要安裝和設定的技術。 您必須在內容伺服器 (如 Web 和檔案伺服器) 上執行 Windows PowerShell BranchCache 命令，但您不需要以任何其他方式變更或重新設定內容伺服器。 此外，您必須設定用戶端電腦使用群組原則，您會在 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 執行 AD DS 的網域控制站上。

### <a name="branchcache"></a>BranchCache

BranchCache 是廣域網路 (WAN) 頻寬最佳化技術，在某些版本的 Windows Server 2016 和 Windows 10 作業系統，以及在某些版本的 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012、 Windows 8 內含、 Windows Server 2008 R2 和 Windows 7。

若要最佳化 WAN 頻寬，使用者存取遠端伺服器上的內容時，BranchCache 會從總公司下載用戶端要求內容或託管雲端內容伺服器和快取在分公司，內容讓其他用戶端電腦若要存取相同的內容，在本機，而非透過 WAN 的分公司。

當您部署託管快取模式的 BranchCache 時，必須在分公司將用戶端電腦設定為託管快取模式用戶端，然後必須在分公司部署託管快取伺服器。 本指南示範如何部署您的託管快取伺服器，包含預先雜湊和預先載入的內容，從您的 Web 和檔案伺服器與\-型內容伺服器。

### <a name="group-policy"></a>群組原則

Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 中的群組原則是用來傳送及套用到一組目標的使用者和電腦的 Active Directory 環境內的一或多個所需的設定或原則設定的基礎結構。 

此基礎結構包含一個群組原則引擎和多個用戶端\-戶端延伸\(Cse\)負責讀取目標用戶端電腦上的原則設定。

此案例使用群組原則來設定處於 BranchCache 託管快取模式的網域成員用戶端電腦。

若要繼續進行本指南，請參閱[BranchCache 託管快取模式部署概觀](2-Bc-Hcm-Deploy-Overview.md)。
