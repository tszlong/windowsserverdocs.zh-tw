---
title: Active Directory 伺服器的效能調整
description: Active Directory 伺服器的效能調整
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab; v-tea
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9fa01e7336953c4b2817535d1e50c0200b93e4fd
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866634"
---
# <a name="performance-tuning-active-directory-servers"></a>Active Directory 伺服器的效能調整

Active Directory 的效能調整著重在兩個目標：
- 已以最佳化方式設定 Active Directory 以儘可能使用最有效率的方式處理負載
- 提交到 Active Directory 的工作負載應該儘可能地有效率

這要求您必須適當地注意三個不同的方面：
- 適當的容量規劃 – 確保有足夠的硬體可支援現有的負載
- 伺服器端調整 – 設定網域控制站以儘可能使用有效率的方式處理負載
- Active Directory 用戶端/應用程式調整 – 確保用戶端與應用程式以最佳化方式使用 Active Directory

## <a name="start-with-capacity-planning"></a>從容量規劃開始

適當地在正確地區的正確的網域中部署數目足夠的網域控制站，而且容納備援能力對於確保以即時方式處理用戶端要求而言非常重要。 這是深入的主題，因此不在此指南的討論範圍內。 我們建議讀者透過閱讀並了解 [Active Directory Domain Services 的容量規劃](capacity-planning-for-active-directory-domain-services.md) (英文) 中所述的建議與指導方針，以開始調整其 Active Directory 效能。

>[!Important]
> 適當地設定並調整 Active Directory 大小對於整體系統與工作負載效能而言有顯著的潛在影響。 我們強烈建議讀者透過閱讀 [Active Directory Domain Services 的容量規劃](capacity-planning-for-active-directory-domain-services.md) (英文) 以開始。

## <a name="updates-and-evolving-recommendations"></a>更新及不斷地演變建議

Active Directory 與用戶端效能最佳化中的大量增強功能在最近數代的作業系統中引進，而且仍不斷精進。 我們建議您部署最新版本的平台以獲得優點，包括：

- 提高的穩定性
- 更好的效能
- 更好的記錄與疑難排解工具

不過，我們了解這需要時間而且許多環境都是在無法 100% 採用最新平台的案例中執行。 某些改良功能已新增到舊版平台，而且我們將繼續新增更多功能。

我們鼓勵您關注我們的小組部落格 [Ask the Directory Services Team](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/bg-p/AskDS) (英文)，密切注意有關管理 ADDS 的最新新聞、指導方針與最佳做法。

## <a name="see-also"></a>請參閱

- [AD DS 容量規劃](capacity-planning-for-active-directory-domain-services.md)
- [硬體考量](hardware-considerations.md)
- [記憶體使用量考量](memory-usage-considerations.md)
- [LDAP 考量](ldap-considerations.md)
- [適當地放置網域控制站與站台考量](site-definition-considerations.md)
- [針對 AD DS 效能問題進行疑難排解](troubleshoot.md)  
  
