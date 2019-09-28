---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: 了解 AD DS 設計
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f46cad23e13edcef57bf00113e601069c02cfd4f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402454"
---
# <a name="understanding-ad-ds-design"></a>了解 AD DS 設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

組織可以使用 Windows Server 中的 Active Directory Domain Services （AD DS）來簡化使用者和資源管理，同時建立可擴充、安全且可管理的基礎結構。 您可以使用 AD DS 來管理您的網路基礎結構，包括分公司、Microsoft Exchange Server 和多樹系環境。  
  
AD DS 部署專案包含三個階段：設計階段、部署階段和作業階段。 在設計階段期間，設計小組會為 AD DS 邏輯結構建立一個最符合組織中將使用目錄服務之每個部門需求的設計。 在設計經過核准之後，部署小組會在實驗室環境中測試設計，然後在生產環境中實行設計。 因為測試是由部署小組執行，而且它可能會影響設計階段，所以它是一種與設計和部署重迭的暫時活動。 當部署完成時，營運小組會負責維護目錄服務。  
  
雖然本指南中所提供的 Windows Server AD DS 設計和部署策略是以廣泛的實驗室和試驗計畫測試為基礎，而且在客戶環境中成功執行，但您可能必須自訂您的 AD DS 設計和部署以更符合特定、複雜的環境。
  
- 如需在分公司環境中部署 AD DS 的詳細資訊，請參閱[唯讀網域控制站（RODC）分公司規劃指南](https://go.microsoft.com/fwlink/?LinkId=100207)。  
- 如需在 Exchange 環境中部署 AD DS 的詳細資訊，請參閱[exchange 2007-規劃 Active Directory 一](https://go.microsoft.com/fwlink/?LinkId=88904)文。  
- 如需在多個樹系環境中部署 AD DS 的詳細資訊，請參閱[windows 2000 和 Windows Server 2003 中的多樹系考慮](https://go.microsoft.com/fwlink/?LinkId=88905)一文。  
