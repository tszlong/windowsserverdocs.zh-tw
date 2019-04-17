---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: "了解 AD DS 設計"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 09afe3d19add87327d05bfafba6e0492a278b968
ms.sourcegitcommit: 1c3e6375b2e8eb01cfd299d0e9478fee46905c99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2018
---
# <a name="understanding-ad-ds-design"></a>了解 AD DS 設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

組織可以使用在 Windows Server Active Directory Domain Services (AD DS)，來同時建立延展性、安全且更容易管理基礎結構簡化管理使用者和資源。 您可以使用 AD DS，以管理您的網路基礎結構，包括分公司、Microsoft Exchange Server 和多樹系的環境。  
  
AD DS 部署專案涉及三個階段：設計階段、deployment 階段和作業階段。 設計階段設計團隊建立 AD DS 邏輯結構適合每個分割組織中，將會使用 directory 服務的需要設計。 已核准設計之後，實驗室測試設計部署小組，並再會設計實作 production 環境中。 測試部署小組，執行，可能會影響設計階段，因為它是重疊設計和部署暫時活動。 部署完成時，請作業小組負責維護 directory 服務。  
  
雖然這個節目表中顯示的 Windows Server AD DS 設計和部署策略的基礎全面的實驗室測試 pilot 程式及成功實作客戶環境中的，您可能要自訂您的 AD DS 設計和若要使其更適合特定且複雜環境部署。  
  
-   如需部署 AD DS 分公司環境中的相關資訊，請查看 Read-Only 網域控制站 (RODC) 分支 Office 規劃指南 ([https://go.microsoft.com/fwlink/?LinkId=100207](https://go.microsoft.com/fwlink/?LinkId=100207))。  
  
-   如需部署 AD DS 換貨的環境中相關資訊，查看 2007-規劃 Active Directory ([https://go.microsoft.com/fwlink/?LinkId=88904](https://go.microsoft.com/fwlink/?LinkId=88904))。  
  
-   如需部署 AD DS 多個的樹系環境中相關資訊，查看 Windows 2000 與 Windows Server 2003 中的多個森林考量 ([https://go.microsoft.com/fwlink/?LinkId=88905](https://go.microsoft.com/fwlink/?LinkId=88905))。  
  


