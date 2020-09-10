---
title: 伺服器清理精靈
description: Windows Server Update Services (WSUS) 主題-如何使用伺服器清除嚮導來管理磁碟空間
ms.topic: article
ms.assetid: 7c351797-2716-4442-a668-60d5b4e77751
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 967df91afdf7bab1f6fd4d6c1daabcf04f104d23
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624401"
---
# <a name="the-server-cleanup-wizard"></a>伺服器清理精靈

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

[伺服器清理嚮導] 已整合至使用者介面，可用於協助您管理磁碟空間。 此嚮導可以執行下列作業：

- 移除未使用的更新和更新修訂，移除所有較舊的更新，並更新尚未核准的修訂。

- 刪除未與伺服器連線的電腦，會刪除在三十天內未連接伺服器的所有用戶端電腦。

- 刪除不需要的更新檔案可刪除更新或下游伺服器不需要的所有更新檔案。

- 拒絕過期的更新會拒絕 Microsoft 已過期的所有更新。

- 拒絕取代的更新會拒絕符合下列所有條件的所有更新：

  -   取代的更新不是強制的

  -   已取代的更新已在伺服器上30天或更多

  -   已取代的更新目前不會依任何用戶端的需求回報

  -   已取代的更新未明確部署到電腦群組90天或更多

  -   取代的更新必須經過核准，才能安裝到電腦群組

  > [!WARNING]
  >  在 WSUS 階層中，強烈建議您先在較低的下游/複本 WSUS 伺服器上執行清除程式，然後再向上移動階層。 在每一部下游伺服器上執行清除之前，在任何上游伺服器上不正確地執行清除，可能會導致上游資料庫和下游資料庫中出現的資料不相符。 資料不相符可能會導致上游和下游伺服器之間的同步處理失敗。
  >
  > [!IMPORTANT]
  >  如果您使用「伺服器清除嚮導」移除不必要的內容，則也會移除您從 Microsoft Update 目錄網站下載的所有私用更新檔案。 您必須在執行 [伺服器清理嚮導] 之後重新匯入這些檔案。

如果使用自動核准規則核准更新，則可能仍處於 [已核准] 狀態，而 [伺服器清除嚮導] 將不會移除更新。 若要移除處於核准狀態的自動核准更新，WSUS 系統管理員至少必須以手動方式將已取代更新的核准狀態設定為 [未核准]，如此一來，伺服器清除嚮導才有資格 declination 這些更新。 [伺服器清除] 嚮導會確保已核准較新的更新，且在將更新標示為已拒絕之前，沒有任何用戶端系統仍會依需要報告該更新。




