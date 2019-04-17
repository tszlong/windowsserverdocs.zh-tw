---
title: 部署 BranchCache 裝載快取模式
description: 本指南部署 BranchCache 裝載快取模式執行的 Windows Server 2016 和 Windows 10 電腦上提供指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 326a1f1edfe6cb763a33ebfc8fd5abdd5b6aab3a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>部署 BranchCache 裝載快取模式

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Windows Server 2016 核心網路指南指示計畫和部署所需的正常運作的網路，以及新的 Active Directory 核心元件&reg;在新的樹系的網域。

本指南如何提供部署 BranchCache 一或多個位置 client 的電腦執行 Windows 的 Read\-Only 網域控制站分公司裝載快取模式中的指示核心網路上建置&reg;10、Windows 8.1 或 Windows 8，並加入網域。

>[!IMPORTANT]
>如果您打算部署，或在已部署執行的 Windows Server 2008 R2 的 BranchCache 裝載快取伺服器請勿使用本指南。 本指南部署裝載快取模式與 Windows server 裝載快取伺服器的指示&reg;2016、Windows Server 2012 R2 或 Windows Server 2012。

本指南包含下列各節。

- [必要條件使用本指南](#bkmk_pre)

- [有關本指南](#bkmk_about)

- [未提供哪些本指南](#bkmk_not)

- [技術概觀](#bkmk_tech)

- [BranchCache 裝載快取模式部署概觀](2-Bc-Hcm-Deploy-Overview.md)

- [BranchCache 裝載快取模式部署計劃](3-Bc-Hcm-Plan.md)

- [BranchCache 裝載快取模式部署](4-Bc-Hcm-Deployment.md)

- [其他資源](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>必要條件使用本指南

這是 Windows Server 2016 核心網路指南小幫手指南。 若要部署 BranchCache 裝載快取本指南使用的模式，您必須先執行動作。

- 使用核心網路節目表部署中主要辦公室核心網路或已經有技術核心網路指南提供安裝及網路上正確運作。 這些技術包括 TCP\ 日 IP v4，DHCP、Active Directory Domain Services \(AD DS\)，以及 DNS。

    > [!NOTE]
    > Windows Server 2016[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)Windows Server 2016 技術文件庫中可使用。  

- 部署 BranchCache 內容伺服器執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 中主要辦公室或雲端的資料中心。 如需如何部署 BranchCache 內容伺服器的資訊，請[其他資源](11-Bc-Hcm-additional-resources.md)。

- 建立您的分支 office，您的主要辦公室之間的寬形區域網路 \(WAN\) 連接，必要時，您的雲端資源，利用 virtual 私人網路 \(VPN\)、DirectAccess 或其他連接方法。

- 部署您分公司 client 電腦執行其中一項下列作業系統，提供 BranchCache 背景聰明傳輸服務（位元）、超文字傳輸通訊協定 (HTTP)，以及伺服器訊息區 (SMB) 的支援。
    - Windows 10 企業版
    - Windows 10 教育版
    - Windows 8.1 企業版
    - Windows 8 企業版

>[!NOTE]
>在下列作業系統中，BranchCache 不支援 HTTP 和 SMB 功能，但支援 BranchCache 位元的功能。
>     - Windows 10 專業版、位元支援只
>     - Windows 8.1 專業版，位元支援只
>     - Windows 8 專業版、位元支援只

## <a name="bkmk_about"></a>有關本指南

已有的 Windows Server 2016 核心網路指南或 Windows Server 2012 核心網路節目表中的指示來部署核心網路，網路和系統管理員或先前已部署包括 Active Directory Domain Services \(AD DS\) 網域名稱服務 \(DNS\)、動態主機設定通訊協定 \(DHCP\) 核心網路節目表中包含技術的人的設計目的是本指南並 TCP\ 日 IP v4。

您檢視的每一種技術，此部署案例中所使用的設計和部署指南至於。 這些指南可協助您判斷這個部署案例提供服務和設定，您需要針對您組織的網路。

## <a name="bkmk_not"></a>未提供哪些本指南

本指南不提供概念 BranchCache，包括 BranchCache 模式和功能的相關資訊的資訊。  

本指南不提供了解如何部署 WAN 連接或其他技術分公司例如 DHCP、RODC 或 VPN 伺服器的資訊。

此外，此指南不提供部署裝載快取伺服器時，您應該使用的硬體上指導方針。 也可在您裝載快取的伺服器，但是您必須先判斷，根據工作負載、硬體功能，以及新的 office 大小、是否將特定電腦上安裝 BranchCache 裝載快取伺服器和多少快取配置的磁碟空間執行其他服務和應用程式。  
本指南不提供如何設定執行 Windows 7 的電腦的指示操作。 如果您正在執行 Windows 7 的分公司 client 電腦，您必須設定這些使用不同的本指南 client 電腦正在執行 Windows 10、Windows 8.1 和「Windows 8 所提供的程序。
  
此外，如果您執行 Windows 7 的電腦，您必須設定裝載快取伺服器伺服器的憑證，來 client 電腦信任的憑證授權單位發行。 \ (所有 client 電腦執行 Windows 10、Windows 8.1 或 Windows 8，如果您不需要設定裝載快取伺服器伺服器的憑證。\) 
> [!IMPORTANT]
> 如果您裝載快取的伺服器執行 Windows Server 2008 R2、使用 Windows Server 2008 R2 [BranchCache 部署指南](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx)而本指南部署 BranchCache 裝載快取模式。 適用於群組原則設定該要執行的 Windows 版本的 Windows 7 到 Windows 10 的所有 BranchCache 戶端節目表中所述。 無法本指南使用的步驟來設定電腦正在執行 Windows Server 2008 R2。

## <a name="bkmk_tech"></a>技術概觀

這個系列指南 BranchCache 是您需要安裝和設定的唯一技術。 您必須執行 Windows PowerShell BranchCache 命令內容伺服器，例如網路上的檔案伺服器，不過您不需要變更或重新設定內容伺服器以任何其他方式。 此外，您必須在您的網域控制站的 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 上執行 AD DS 使用群組原則設定 client 的電腦。

### <a name="branchcache"></a>BranchCache

BranchCache 是隨附於某些版本的 Windows Server 2016 和 Windows 10 作業系統、以及於某些版本的 Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2 和 Windows 7 的寬形區域網路 (WAN) 頻寬最佳化技術。

若要最佳化 WAN 的頻寬，當使用者存取 content 遠端伺服器，BranchCache 下載 client 要求 content 從您的主要辦公室或裝載的雲端內容伺服器並快取 content 在分公司的位置，讓在本機而不是透過 WAN 存取相同 content 分公司其他 client 電腦。

當您 BranchCache 部署裝載快取模式時，您必須在分公司設定 client 電腦裝載快取模式戶端，以和您必須再部署分公司裝載快取的伺服器。 本指南示範如何部署 prehashed 和預先載入 content 從您的網頁伺服器您裝載快取，並檔案 server\ 型內容伺服器。

### <a name="group-policy"></a>群組原則

Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 中的群組原則是用來提供及適用於一或多個您想要的設定原則設定目標的使用者和 Active Directory 環境中的電腦的基礎結構。 

這個基礎結構包含的群組原則引擎和多 client\ 端的擴充功能 \(CSEs\) 負責朗讀目標 client 電腦上的原則設定。

群組原則在本案例中用於 BranchCache 裝載快取模式設定的網域成員 client 電腦。

若要繼續使用此快速入門，請查看[BranchCache 裝載快取模式部署概觀](2-Bc-Hcm-Deploy-Overview.md)。
