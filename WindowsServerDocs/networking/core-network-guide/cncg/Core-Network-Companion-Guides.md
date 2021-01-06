---
title: 核心網路附屬指南
description: 本主題概要說明《 Windows Server 2016 核心網路指南》的附屬指南
manager: brianlic
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: ffcfbecfa47c39f0bb6e76a6e34953a2ef9140e7
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950234"
---
# <a name="core-network-companion-guidance"></a>核心網路附屬指導方針

>適用於：Windows Server (半年度管道)、Windows Server 2016

《 Windows Server 2016 [核心網路指南》](../core-network-guide.md) 提供如何 &reg; 使用新的根域和支援的網路基礎結構部署新的 Active Directory 樹系的指示，隨附指南提供您將功能新增至網路的功能。

每個附屬指南都能讓您在部署核心網路之後完成特定的目標。 在某些情況下會提供多個附屬指南，當以正確的順序一起部署時，可以讓您用可衡量、符合成本效益及合理的方式，完成非常複雜的目標。

如果您在發現核心網路指南之前就已經部署 Active Directory 網域和核心網路，仍然可以使用附屬指南來新增網路功能。 只要使用核心網路指南做為先決條件清單，並了解若要利用附屬指南部署其他功能，您的網路必須符合核心網路指南所提供的先決條件。

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>核心網路附屬指南：部署 802.1 X 有線和無線部署的伺服器憑證

本附屬指南說明如何針對執行網路原則伺服器 \( NPS \) 、遠端存取服務 \( RAS \) 或兩者的電腦，部署伺服器憑證，以在核心網路上建立。

當您部署以可延伸驗證通訊協定 \( eap \) 和受保護 \( 的 eap PEAP \) 進行網路存取驗證的憑證型驗證方法時，需要伺服器憑證。 使用 \( \) EAP 和 PEAP 憑證驗證方法 Active Directory 憑證服務 AD CS 來部署伺服器憑證提供下列優點：

- 將 NPS 或 RAS 伺服器的身分識別系結至私密金鑰
- 以符合成本效益且安全的方法，將憑證自動註冊到網域成員 NPS 和 RAS 伺服器
- 管理憑證與憑證授權單位的有效方法
- 憑證式驗證所提供的安全性
- 基於其他目的擴展憑證用途的能力

如需有關如何部署伺服器憑證的指示，請參閱 [部署 802.1 x 有線和無線部署的伺服器憑證](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md)。
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>核心網路附屬指南：部署 Password-Based 802.1 X 驗證無線存取

本附屬指南說明如何 \( \) 使用「受保護802.11 的可延伸 \- 驗證通訊協定」 \-Microsoft 挑戰交握驗證通訊協定第2版 \( PEAP \- MS \- CHAP v2 \) ，透過核心網路建立。

驗證方法： PEAP \- MS \- CHAP v2 需要驗證執行網路原則伺服器 NPS 的 \( 伺服器 \) 使用伺服器憑證來呈現無線用戶端，以證明 NPS 身分識別給用戶端，不過使用者驗證不是使用憑證來執行，而是使用者提供其網域使用者名稱和密碼。

因為 PEAP \- MS \- CHAP v2 需要使用者在驗證程式期間提供密碼型認證，而不是憑證，所以部署的部署通常比 EAP \- tls 或 PEAP tls 更簡單且成本更低 \- 。

使用本指南來部署使用 PEAP \- MS \- CHAP v2 驗證方法的無線存取之前，您必須執行下列動作：

1. 遵循《核心網路指南》中的指示來部署核心網路基礎結構，或已經在網路上部署該指南中所提供的技術。
2. 遵循核心網路附屬指南中的指示，部署 802.1 X 有線和無線部署的伺服器憑證，或已經在網路上部署該指南中所提供的技術。

如需有關如何使用 PEAP MS CHAP v2 部署無線存取的指示 \- \- ，請參閱 [部署 Password-Based 802.1 x 驗證無線存取](wireless/a-deploy-8021X-wireless-access.md)。

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>核心網路附屬指南：部署 BranchCache 託管快取模式

本附屬指南說明如何在一或多個分公司以託管快取模式部署 BranchCache。

BranchCache 是一種廣域網路 (WAN) 頻寬優化技術，包含在某些版本的 Windows Server 2016 和 Windows 10 作業系統，以及舊版 Windows 和 Windows Server 中。

以託管快取模式部署 BranchCache 時，會在稱為託管快取伺服器的一或多部伺服器電腦上託管分公司的內容快取。 託管快取伺服器除了裝載快取之外，還可以執行工作負載，這可讓您在分公司將伺服器用於多個用途。

BranchCache 託管快取模式會提高快取效率，因為即使最初要求和快取資料的用戶端離線，也可以使用內容。 因為託管快取伺服器永遠可供使用，所以快取的內容越多，節省的 WAN 頻寬就越多，進而改善 BranchCache 的效率。

當您部署託管快取模式時，多重子網分公司中的所有用戶端都可以存取儲存在託管快取伺服器上的單一快取，即使用戶端位於不同的子網也一樣。

如需有關如何以託管快取模式部署 BranchCache 的指示，請參閱 [部署 Branchcache 託管](bc-hcm/1-Deploy-Bc-Hcm.md)快取模式。