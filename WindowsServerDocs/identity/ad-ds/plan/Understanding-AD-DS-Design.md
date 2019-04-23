---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: 了解 AD DS 設計
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c94f6ddd19e3178243545b0cc71f6f4c7bb4dbec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828789"
---
# <a name="understanding-ad-ds-design"></a>了解 AD DS 設計

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

組織可以使用 Windows server Active Directory 網域服務 (AD DS)，以簡化使用者和資源管理建立可擴充、 安全且可管理的基礎結構時。 您可以使用 AD DS 來管理您的網路基礎結構，包括分公司辦公室、 Microsoft Exchange Server，以及多個樹系環境。  
  
AD DS 部署專案包含三個階段： 設計階段，部署階段，以及作業階段。 在設計階段中，設計團隊會建立最符合需求的組織，將會使用目錄服務中的每個部門的 AD DS 邏輯結構的設計。 設計經核准後，部署小組在實驗室環境中測試的設計，並再與生產環境中實作的設計。 因為測試由部署團隊的方式來執行，而且可能會影響設計階段，它會是暫時重疊設計和部署活動。 部署完成時，作業小組會負責維護目錄服務。  
  
雖然本指南中提供的 Windows Server AD DS 設計與部署策略根據廣泛的實驗室和測試試驗計畫，以及在客戶環境中的成功實作而定，您可能要自訂您的 AD DS 設計與若要使其更符合特定的複雜環境的部署。
  
- 如需分公司環境中部署 AD DS 的詳細資訊，請參閱[唯讀網域控制站 (RODC) 分公司規劃指南](https://go.microsoft.com/fwlink/?LinkId=100207)。  
- 如需有關如何部署 Exchange 環境中的 AD DS 的詳細資訊，請參閱[Exchange 2007-規劃 Active Directory](https://go.microsoft.com/fwlink/?LinkId=88904)。  
- 如需有關多樹系環境中部署 AD DS 的詳細資訊，請參閱[Windows 2000 和 Windows Server 2003 中的多個樹系考量](https://go.microsoft.com/fwlink/?LinkId=88905)。  
