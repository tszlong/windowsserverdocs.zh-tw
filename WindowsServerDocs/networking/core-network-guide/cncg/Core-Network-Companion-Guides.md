---
title: 核心網路附屬指南
description: 本主題概要說明《 Windows Server 2016 核心網路指南》的附屬指南
manager: brianlic
ms.technology: networking
ms.prod: windows-server
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c0895cfd62d462ef6d158dc39ef59a9ee10a7c98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406310"
---
# <a name="core-network-companion-guidance"></a>核心網路附屬指導方針

>適用於：Windows Server (半年通道)、Windows Server 2016

雖然《 Windows Server 2016[核心網路指南》](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)提供如何使用新的根域和支援的網路基礎結構來部署新的 Active Directory&reg; 樹系的指示，但附屬指南可讓您將功能新增至您的網路。

每個附屬指南都能讓您在部署核心網路之後完成特定的目標。 在某些情況下會提供多個附屬指南，當以正確的順序一起部署時，可以讓您用可衡量、符合成本效益及合理的方式，完成非常複雜的目標。

如果您在發現核心網路指南之前就已經部署 Active Directory 網域和核心網路，仍然可以使用附屬指南來新增網路功能。 只要使用核心網路指南做為先決條件清單，並了解若要利用附屬指南部署其他功能，您的網路必須符合核心網路指南所提供的先決條件。

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>核心網路附屬指南：部署 802.1 X 有線和無線部署的伺服器憑證 

本附屬指南說明如何藉由針對執行網路原則伺服器 \(NPS\)、遠端存取服務 \(RAS\)或兩者的電腦，部署伺服器憑證來建立核心網路。

當您使用可延伸的驗證通訊協定來部署以憑證為基礎的驗證方法時，需要伺服器憑證 \(EAP\) 和受保護的 EAP \(PEAP\) 進行網路存取驗證。 使用 Active Directory 憑證服務部署伺服器憑證 \(AD CS\) 針對 EAP 和 PEAP 憑證型驗證方法提供下列優點：

- 將 NPS 或 RAS 伺服器的身分識別系結至私密金鑰
- 以符合成本效益且安全的方式，自動向網域成員 NPS 和 RAS 伺服器註冊憑證
- 管理憑證與憑證授權單位的有效方法
- 憑證式驗證所提供的安全性
- 基於其他目的擴展憑證用途的能力
  
如需如何部署伺服器憑證的指示，請參閱[部署 802.1 x 有線和無線部署的伺服器憑證](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md)。  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>核心網路附屬指南：部署以密碼為基礎的 802.1 X 驗證無線存取

本附屬指南說明如何使用受保護的可延伸驗證通訊協定 \ – Microsoft 挑戰交握驗證通訊協定第2版 \(PEAP\-MS\-CHAP v2\)，在核心網路上建立 \(IEEE\) 802.1 X\-驗證的 IEEE 802.11 無線存取的相關指示。

驗證方法 PEAP\-MS\-CHAP v2 需要驗證執行網路原則伺服器的伺服器 \(NPS\) 以伺服器憑證呈現無線用戶端，以向用戶端證明 NPS 身分識別，但是使用者驗證不會使用憑證執行，而是由使用者提供其網域使用者名稱和密碼。

因為 PEAP\-MS\-CHAP v2 要求使用者在驗證程式期間提供密碼型認證，而不是憑證，所以部署的成本通常比 EAP\-TLS 或 PEAP\-TLS 更容易且較便宜。

使用本指南部署具有 PEAP\-MS\-CHAP v2 驗證方法的無線存取之前，您必須執行下列動作：

1. 遵循《核心網路指南》中的指示來部署您的核心網路基礎結構，或已在網路上部署該指南中所提供的技術。
2. 依照核心網路附屬指南部署 802.1 X 有線和無線部署的伺服器憑證中的指示，或已在網路上部署該指南中所提供的技術。

如需有關如何使用 PEAP\-MS\-CHAP v2 部署無線存取的指示，請參閱[部署以密碼為基礎的 802.1 x 驗證無線存取](wireless/a-deploy-8021X-wireless-access.md)。

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>核心網路附屬指南：部署 BranchCache 託管快取模式

本附屬指南說明如何在一或多個分公司的託管快取模式中部署 BranchCache。

BranchCache 是一種廣域網路（WAN）頻寬優化技術，包含在某些 Windows Server 2016 和 Windows 10 作業系統版本中，以及舊版 Windows 和 Windows Server 中。

以託管快取模式部署 BranchCache 時，會在稱為託管快取伺服器的一或多部伺服器電腦上託管分公司的內容快取。 託管快取伺服器除了裝載快取之外，還可以執行工作負載，這可讓您將伺服器用於分公司中的多個用途。

BranchCache 託管快取模式會增加快取效率，因為即使原先要求和快取資料的用戶端已離線，也可以使用內容。 因為託管快取伺服器永遠可供使用，所以快取的內容越多，節省的 WAN 頻寬就越多，進而改善 BranchCache 的效率。

當您部署託管快取模式時，多重子網分公司中的所有用戶端都可以存取儲存在託管快取伺服器上的單一快取，即使用戶端位於不同的子網也一樣。

如需如何在託管快取模式中部署 BranchCache 的指示，請參閱[部署 Branchcache 託管](bc-hcm/1-Deploy-Bc-Hcm.md)快取模式。
