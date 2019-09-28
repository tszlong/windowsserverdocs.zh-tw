---
title: 伺服器清理精靈
description: Windows Server Update Service （WSUS）主題-如何使用伺服器清理嚮導來管理磁碟空間
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c351797-2716-4442-a668-60d5b4e77751
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e285e59a27b6bf0ef1bf3b1ab0f78a96efa60c87
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361530"
---
# <a name="the-server-cleanup-wizard"></a>伺服器清理精靈

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

伺服器清理 Wizard 已整合到使用者介面中，可用來協助您管理磁碟空間。 此 wizard 可以執行下列作業：

- 移除未使用的更新和更新修訂，移除尚未核准的所有舊版更新和更新修訂。

- 刪除未連線到伺服器的電腦刪除在三十天或以上的所有未與伺服器連線的用戶端電腦。

- 刪除不需要的更新檔案刪除更新或下游伺服器不需要的所有更新檔案。

- 拒絕過期的更新會拒絕所有已由 Microsoft 到期的更新。

- 拒絕被取代的更新會拒絕符合下列所有準則的所有更新：

  -   已取代的更新不是必要的

  -   已取代的更新已在伺服器上30天或更多

  -   任何用戶端目前都未回報已取代的更新

  -   已取代的更新未明確部署到電腦群組90天或更多

  -   取代更新必須經過核准才能安裝到電腦群組

  > [!WARNING]
  >  在 WSUS 階層中，強烈建議您先在較低的下游/複本 WSUS 伺服器上執行清除程式，然後再向上移動階層。 在每一部下游伺服器上執行清除之前，在任何上游伺服器上不正確執行清除，可能會導致上游資料庫和下游資料庫中出現的資料不相符。 資料不符可能會導致上游和下游伺服器之間的同步處理失敗。 
  > 
  > [!IMPORTANT]
  >  如果您使用 [伺服器清理] Wizard 移除不必要的內容，則也會移除您從 Microsoft Update 目錄網站下載的所有私人更新檔案。 執行 [伺服器清理嚮導] 之後，您必須重新匯入這些檔案。 

如果使用自動核准規則核准更新，它們可能仍處於「已核准」狀態，且不會被伺服器清理嚮導移除。 若要移除處於「已核准」狀態的自動核准更新，WSUS 系統管理員必須至少以手動方式將已取代更新的核准狀態設定為「未核准」，這樣他們就可以由 [伺服器清理] Wizard 進行 declination。 伺服器清理嚮導會確保已核准較新的更新，而且沒有任何用戶端系統在將更新標示為「已拒絕」之前，不會視需要進行更新。




