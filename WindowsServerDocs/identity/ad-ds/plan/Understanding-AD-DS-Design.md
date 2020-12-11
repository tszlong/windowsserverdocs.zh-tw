---
description: 深入瞭解：瞭解 AD DS 設計
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: 了解 AD DS 設計
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: fd31da84f722f07bededbfd4af7760e226b6f574
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049176"
---
# <a name="understanding-ad-ds-design"></a>了解 AD DS 設計

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

組織可以使用 Windows Server 中 Active Directory Domain Services (AD DS) ，以簡化使用者和資源管理，同時建立可擴充、安全且可管理的基礎結構。 您可以使用 AD DS 來管理您的網路基礎結構，包括分公司、Microsoft Exchange Server 和多個樹系環境。

AD DS 部署專案包含三個階段：設計階段、部署階段和作業階段。 在設計階段期間，設計團隊會建立 AD DS 邏輯結構的設計，最符合將使用目錄服務的組織中每個部門的需求。 在設計獲得核准之後，部署團隊會在實驗室環境中測試設計，然後在生產環境中實行設計。 由於測試是由部署小組執行，而且可能會影響到設計階段，這是同時重迭設計和部署的過渡活動。 當部署完成時，營運小組會負責維護目錄服務。

雖然本指南所提供的 Windows Server AD DS 設計和部署策略是以廣泛的實驗室和試驗計畫測試，以及在客戶環境中成功執行的方式為基礎，但是您可能必須自訂您的 AD DS 設計和部署，以更符合特定、複雜的環境。

- 如需在分公司環境中部署 AD DS 的詳細資訊，請參閱《 [唯讀網域控制站 (RODC) 分公司規劃指南》](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd734758(v=ws.10))。
- 如需在 Exchange 環境中部署 AD DS 的詳細資訊，請參閱 [Exchange Server 組織中](/exchange/plan-and-deploy/active-directory/active-directory)的文章 Active Directory。
- 如需在多個樹系環境中部署 AD DS 的詳細資訊，請參閱 [windows 2000 和 Windows Server 2003 中的多個樹系考慮](/previous-versions/windows/it-pro/windows-server-2003/cc739395(v=ws.10))一文。
