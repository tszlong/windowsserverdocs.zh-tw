---
title: 核心網路附屬指南
description: 本主題提供 Windows Server 2016 核心網路指南的附屬指南概觀
manager: brianlic
ms.technology: networking
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b757e1914ee263a041f39e9767d3cb8af38403dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816799"
---
# <a name="core-network-companion-guidance"></a>核心網路附屬指導方針

>適用於：Windows Server （半年通道），Windows Server 2016

雖然 Windows Server 2016[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)提供如何部署新的 Active Directory 中的指示&reg;樹系中使用新的根網域和支援網路基礎結構，附屬指南提供您若要將功能加入至您網路的能力。

每個附屬指南都能讓您在部署核心網路之後完成特定的目標。 在某些情況下會提供多個附屬指南，當以正確的順序一起部署時，可以讓您用可衡量、符合成本效益及合理的方式，完成非常複雜的目標。

如果您在發現核心網路指南之前就已經部署 Active Directory 網域和核心網路，仍然可以使用附屬指南來新增網路功能。 只要使用核心網路指南做為先決條件清單，並了解若要利用附屬指南部署其他功能，您的網路必須符合核心網路指南所提供的先決條件。

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>核心網路附屬指南：部署 802.1 X 有線和無線部署的伺服器憑證 

本附屬指南說明如何建置在核心網路部署執行網路原則伺服器的電腦的伺服器憑證\(NPS\)，遠端存取服務\(RAS\)，或兩者。

當您部署具有可延伸驗證通訊協定的憑證型驗證方法時，會需要伺服器憑證\(EAP\)和 受保護的 EAP \(PEAP\)進行網路存取驗證。 部署與 Active Directory 憑證服務的伺服器憑證\(AD CS\) EAP 與 PEAP 憑證式驗證方法提供下列優點：

- 繫結至的私用金鑰的 NPS 或 RAS 伺服器的身分識別
- 自動註冊憑證至網域成員 NPS 與 RAS 伺服器符合成本效益且安全的方法
- 管理憑證與憑證授權單位的有效方法
- 憑證式驗證所提供的安全性
- 基於其他目的擴展憑證用途的能力
  
如需有關如何部署伺服器憑證的指示，請參閱 <<c0> [ 部署 802.1x 有線和無線部署的伺服器憑證](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md)。  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>核心網路附屬指南：部署密碼型 802.1X 驗證無線存取

本附屬指南說明如何建置在核心網路時所提供的指示，有關如何部署美國電機暨電子工程師\(IEEE\) 802.1x\-驗證 IEEE 802.11 無線存取使用受保護的可延伸驗證 Protocol\ – Microsoft Challenge Handshake 驗證通訊協定第 2 版\(PEAP\-MS\-MS-CHAP v2\)。

驗證方法 PEAP\-MS\-MS-CHAP v2，需要該驗證執行網路原則伺服器的伺服器\(NPS\)證明 NPS 身分識別，以伺服器憑證會向無線用戶端用戶端，但不執行使用者驗證使用憑證-相反地，使用者提供其網域使用者名稱和密碼。

因為 PEAP\-MS\-MS-CHAP v2 要求使用者提供密碼認證，而不是憑證驗證程序期間，通常更容易且成本較低比 EAP 部署\-TLS 或 PEAP\-TLS。

部署 PEAP 與無線存取的情況下，您在使用本指南之前\-MS\-CHAP v2 驗證方法，您必須執行下列動作：

1. 請遵循在核心網路指南 》 中的指示部署核心網路基礎結構，或已有的技術指南中呈現該在網路上部署。
2. 遵循 802.1x 有線和無線部署核心網路附屬指南部署伺服器憑證的指示，或已有的技術指南中呈現該在網路上部署。

如需有關如何部署無線存取與 PEAP\-MS\-CHAP v2，請參閱[部署密碼型 802.1x 驗證無線存取](wireless/a-deploy-8021X-wireless-access.md)。

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>核心網路附屬指南：部署 BranchCache 託管快取模式

本附屬指南說明如何部署一或多個分公司中託管快取模式中的 BranchCache。

BranchCache 是廣域網路 (WAN) 頻寬最佳化技術，就會包含在某些版本的 Windows Server 2016 和 Windows 10 作業系統，以及在舊版的 Windows 和 Windows Server。

以託管快取模式部署 BranchCache 時，會在稱為託管快取伺服器的一或多部伺服器電腦上託管分公司的內容快取。 託管快取伺服器可以執行除了裝載快取，可讓您針對分公司中的多種目的使用伺服器工作負載。

BranchCache 託管快取模式會增加快取效率，因為內容可用，即使原來要求和快取資料的用戶端處於離線狀態。 因為託管快取伺服器永遠可供使用，所以快取的內容越多，節省的 WAN 頻寬就越多，進而改善 BranchCache 的效率。

當您部署託管快取模式時，多個子網路的分公司中的所有用戶端可以存取儲存在託管快取伺服器上，即使在用戶端位於不同的子網路上的單一快取。

如需有關如何部署 BranchCache 託管快取模式中的指示，請參閱 <<c0> [ 部署 BranchCache 託管快取模式](bc-hcm/1-Deploy-Bc-Hcm.md)。
