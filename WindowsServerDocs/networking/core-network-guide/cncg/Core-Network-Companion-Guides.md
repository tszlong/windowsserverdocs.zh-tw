---
title: 核心網路小幫手指南
description: 本主題提供 Windows Server 2016 核心網路指南小幫手指南的概觀
manager: brianlic
ms.technology: networking
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3c272c51cc69017b75e50e79e58186c0ea7c6391
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-companion-guides"></a>核心網路小幫手指南

>適用於：Windows Server（以每年次管道）、Windows Server 2016

在 Windows Server 2016[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)上部署新的 Active Directory 的方式指示&reg;的新根網域和支援網路基礎結構，小幫手指南樹系為您提供的功能來將功能新增至您的網路。

每個小幫手指南可讓您以完成部署核心網路後的特定的目標。 有時候，有多個小幫手引導，當部署，以正確的順序，讓您完成複雜的目標測量、效益合理的方式。

如果您之前遇到核心網路指南部署 Active Directory domain 和 core 網路，您可以使用小幫手指南將功能新增至您的網路。 只要使用核心網路節目表清單的必要條件，以和知道您的部署小幫手指南使用的其他功能，您的網路必須符合所提供的節目表核心網路的必要條件。

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>核心網路小幫手指南：部署 802.1 X 有線與 Wireless 部署伺服器的憑證 

這個小幫手指南如何建置核心網路部署電腦正在執行的網路原則伺服器 \(NPS\)、\(RAS\) 遠端存取服務，或兩者伺服器的憑證。

部署憑證式的驗證方法受保護的 EAP \(PEAP\) 延伸驗證通訊協定 \(EAP\) 與網路存取驗證時，所需伺服器的憑證。 部署伺服器的憑證的 Active Directory 憑證服務 EAP 和 PEAP 憑證為基礎的驗證方法 \(AD CS\) 提供下列優點：

- 繫結私密金鑰 NPS 或 RAS 伺服器的身分
- 自動註冊網域成員 NPS 及遠端存取伺服器的憑證成本效益且安全的方法
- 管理憑證和憑證授權單位有效的方法
- 所提供的認證驗證安全性
- 展開憑證用於其他用途的能力
  
如需部署伺服器的憑證的方式指示，請查看[適用於 802.1 X 的有線和無線部署部署伺服器憑證](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md)。  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>核心網路小幫手指南：部署密碼為基礎的 802.1 X 驗證 Wireless 存取

這個小幫手指南如何建置提供有關如何將協會和電子工程師 \(IEEE\) 802.1X\ 部署核心網路-驗證 IEEE 802.11 wireless 存取使用保護延伸驗證 Protocol\ – Microsoft 挑戰交換驗證通訊協定第 2 \ (PEAP\-MS\-CHAP v2\)。

驗證方法 PEAP\-MS\-CHAP v2 需要的驗證執行伺服器的憑證，以證明 NPS 伺服器身分 client 的網路原則伺服器 \(NPS\) 出現 wireless 戶端伺服器，但是驗證使用者無法使用憑證來執行-反而使用者提供網域使用者名稱和密碼。

PEAP\-MS\-CHAP v2 需要使用者驗證程序期間密碼認證，而非憑證提供，因為它是通常會更簡單且更比 EAP\ TLS 或 PEAP\ TLS 部署。

本指南使用的 PEAP\-MS\-CHAP v2 驗證方法部署 wireless 存取之前，您必須執行下列動作：

1. 依照指示核心網路節目表中的部署核心網路基礎結構，或是已經有技術顯示在網路上部署該指南。
2. 請依照核心網路小幫手指南部署伺服器的憑證 802.1 X 的有線和無線部署，或已經有技術顯示在網路上部署該指南。

如需如何部署 PEAP\-MS\-CHAP v2 wireless 存取指示，請查看[架構部署密碼 802.1 X 驗證 Wireless 存取](wireless/a-deploy-8021X-wireless-access.md)。

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>核心網路小幫手指南：部署 BranchCache 裝載快取模式

這個小幫手指南如何部署 BranchCache 一或多個分公司裝載快取模式。

BranchCache 是隨附於某些版本的 Windows Server 2016 和 Windows 10 作業系統，以及在舊版 Windows 和 Windows Server 的寬形區域網路 (WAN) 頻寬最佳化技術。

當您在裝載快取模式部署 BranchCache 時，裝載內容快取分公司在上一個或更多伺服器電腦，稱為「裝載快取的伺服器。 工作負載除了裝載快取，可讓您使用多個用途分公司伺服器執行裝載快取的伺服器。

BranchCache 裝載快取模式增加效率快取因為 content 使用即使離線 client 原始要求而且快取的資料。 因為都可使用裝載快取伺服器，更多 content 快取，可提供更大節省 WAN 的頻寬，並改進 BranchCache 效率。

當您部署裝載快取模式時，分公司多子網路中的所有戶端可以都存取儲存在裝載快取伺服器上，即使戶端不同子網路上的單一快取。

如何部署 BranchCache 裝載快取模式中的指示，請查看[部署 BranchCache 裝載快取模式](bc-hcm/1-Deploy-Bc-Hcm.md)。
