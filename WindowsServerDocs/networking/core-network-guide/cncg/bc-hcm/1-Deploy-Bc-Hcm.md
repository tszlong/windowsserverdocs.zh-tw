---
title: 部署 BranchCache 託管快取模式
description: 藉由提供在一或多個分公司中以託管快取模式部署 BranchCache 的指示，瞭解如何在一或多個分公司使用 Read-Only 網域控制站，其中用戶端電腦 Windows 10、Windows 8.1 或 Windows 8，並且已加入網域。
manager: brianlic
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 51d8b27d77ccc47d4d7ad1a460da88f6be8339b9
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965620"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>部署 BranchCache 託管快取模式

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

《 Windows Server 2016 核心網路指南》提供在新樹系中規劃和部署全功能網路與新 Active Directory 網域所需之核心元件的指示 &reg; 。

本指南說明如何在核心網路上建立，方法是提供在一或多個分公司中以託管快取模式部署 BranchCache 的指示， \- 其中用戶端電腦執行的是 Windows &reg; 10、Windows 8.1 或 Windows 8，並已加入網域。

>[!IMPORTANT]
>如果您打算部署或已部署執行 Windows Server 2008 R2 的 BranchCache 託管快取伺服器，請勿使用本指南。 本指南提供的指示說明如何使用執行 Windows Server &reg; 2016、Windows server 2012 R2 或 Windows server 2012 的託管快取伺服器部署託管快取模式。

本指南涵蓋下列各節。

- [使用本指南的必要條件](#bkmk_pre)

- [關於本指南](#bkmk_about)

- [本指南中未提供的內容](#bkmk_not)

- [技術概覽](#bkmk_tech)

- [BranchCache 託管快取模式部署總覽](2-Bc-Hcm-Deploy-Overview.md)

- [BranchCache 託管快取模式部署規劃](3-Bc-Hcm-Plan.md)

- [BranchCache 託管快取模式部署](4-Bc-Hcm-Deployment.md)

- [其他資源](11-Bc-Hcm-additional-resources.md)

## <a name="prerequisites-for-using-this-guide"></a><a name="bkmk_pre"></a>使用本指南的必要條件

這是《 Windows Server 2016 核心網路指南》的附屬指南。 若要使用此指南部署託管快取模式的 BranchCache，您必須先執行以下作業：

- 使用《核心網路指南》在總公司部署核心網路，或是在您的網路上已經安裝《核心網路指南》中提供的技術且正常運作。 這些技術包括 TCP \/ IP v4、DHCP、Active Directory Domain Services \( AD DS \) 和 DNS。

    > [!NOTE]
    > Windows server 2016 [核心網路指南](../../core-network-guide.md) 適用于 windows Server 2016 技術文件庫。

- 在總公司或雲端資料中心部署執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的 BranchCache 內容伺服器。 如需有關如何部署 BranchCache 內容伺服器的詳細資訊，請參閱 [其他資源](11-Bc-Hcm-additional-resources.md)。

- \( \) 使用虛擬私人網路 \( VPN \) 、DirectAccess 或其他連接方法，在分公司、總公司和您的雲端資源之間建立廣域網路 WAN 連線。

- 在分公司中部署執行下列其中一個作業系統的用戶端電腦，以提供 BranchCache 支援背景智慧型傳送服務 (位) 、超文字傳輸通訊協定 (HTTP) 和伺服器訊息區 (SMB) 。
    - Windows 10 Enterprise
    - Windows 10 Education
    - Windows 8.1 Enterprise
    - Windows 8 企業版

> [!NOTE]
> 在下列作業系統中，BranchCache 不支援 HTTP 和 SMB 功能，但支援 BranchCache BITS 功能。
>     - Windows 10 專業版，僅支援 BITS
>     - Windows 8.1 Pro、BITS 僅支援
>     - Windows 8 Pro、BITS 僅支援

## <a name="about-this-guide"></a><a name="bkmk_about"></a>關於本指南

本指南是針對已遵循《 Windows Server 2016 核心網路指南》或 Windows Server 2012 核心網路指南中的指示來部署核心網路的網路和系統管理員所設計，或針對先前已部署核心網路指南中所含技術的使用者（包括 Active Directory Domain Services \( AD DS \) 、功能變數名稱服務 \( DNS \) 、動態主機設定通訊協定 \( DHCP \) 和 TCP \/ IP v4）所設計。

建議您檢閱此部署案例中所用每個技術的設計和部署指南。 這些指南可協助您判斷此部署案例是否提供貴組織的網路需要的服務與設定。

## <a name="what-this-guide-does-not-provide"></a><a name="bkmk_not"></a>本指南中未提供的內容

本指南未提供 BranchCache 的概念資訊，包括關於 BranchCache 模式和功能的資訊。

本指南未提供如何在分公司部署 WAN 連線或其他技術 (如 DHCP、RODC 或 VPN 伺服器) 的資訊。

此外，本指南未提供部署託管快取伺服器應使用之硬體的相關指引。 您可以在託管快取伺服器上執行其他服務和應用程式，但是您必須根據工作負載、硬體功能、分公司規模大小等因素進行判斷，是否要在特定電腦上安裝 BranchCache 託管快取伺服器，以及要為快取配置多少磁碟空間。
本指南不提供設定執行 Windows 7 之電腦的指示。 如果您的分公司有執行 Windows 7 的用戶端電腦，您必須使用與本指南中所提供的程式不同的程式來設定這些電腦，以供執行 Windows 10、Windows 8.1 和 Windows 8 的用戶端電腦使用。

此外，如果您有執行 Windows 7 的電腦，就必須使用由用戶端電腦信任的憑證授權單位單位所發行的伺服器憑證來設定託管快取伺服器。 \(如果您的所有用戶端電腦都是執行 Windows 10、Windows 8.1 或 Windows 8，則不需要使用伺服器憑證來設定託管快取伺服器。\)
> [!IMPORTANT]
> 如果託管快取伺服器執行的是 Windows Server 2008 R2，請使用《 Windows Server 2008 R2 [BranchCache 部署指南》](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee649232(v=ws.10)) （而非本指南）來部署託管快取模式的 branchcache。 將該指南中所述的群組原則設定套用到執行 windows 7 版 Windows 的所有 BranchCache 用戶端，以 Windows 10。 無法使用本指南中的步驟來設定執行 Windows Server 2008 R2 的電腦。

## <a name="technology-overviews"></a><a name="bkmk_tech"></a>技術概覽

本附屬指南中，BranchCache 是您唯一需要安裝和設定的技術。 您必須在內容伺服器 (如 Web 和檔案伺服器) 上執行 Windows PowerShell BranchCache 命令，但您不需要以任何其他方式變更或重新設定內容伺服器。 此外，您必須使用在 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 上執行 AD DS 的網域控制站上的群組原則，來設定用戶端電腦。

### <a name="branchcache"></a>BranchCache

BranchCache 是一種廣域網路 (WAN) 頻寬優化技術，包含在某些版本的 Windows Server 2016 和 Windows 10 作業系統，以及某些 Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2 和 Windows 7 的版本中。

當使用者存取遠端伺服器上的內容時，若要將 WAN 頻寬優化，BranchCache 會從總公司或託管雲端內容伺服器下載用戶端要求的內容，並快取分公司位置的內容，讓分公司的其他用戶端電腦能夠在本機存取相同的內容，而不是透過 WAN。

當您部署託管快取模式的 BranchCache 時，必須在分公司將用戶端電腦設定為託管快取模式用戶端，然後必須在分公司部署託管快取伺服器。 本指南示範如何使用 prehashed 和預先載入的內容，從您的 Web 和以檔案伺服器為基礎的內容伺服器部署託管快取伺服器 \- 。

### <a name="group-policy"></a>群組原則

Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的群組原則是一種基礎結構，可用來將一或多個所需的設定或原則設定，提供給 Active Directory 環境內的一組目標使用者和電腦。

此基礎結構包含群組原則引擎和多個用戶端 \- 延伸 \( cse \) ，負責讀取目標用戶端電腦上的原則設定。

此案例使用群組原則來設定處於 BranchCache 託管快取模式的網域成員用戶端電腦。

若要繼續進行本指南，請參閱 [BranchCache 託管快取模式部署總覽](2-Bc-Hcm-Deploy-Overview.md)。