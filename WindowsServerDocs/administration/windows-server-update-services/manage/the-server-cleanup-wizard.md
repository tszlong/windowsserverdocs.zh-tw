---
title: 伺服器清理精靈
description: Windows Server Update Service (WSUS) 主題-如何使用伺服器清理精靈 來管理磁碟空間
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9d9287bb8bfc0fd51c53c598ccbc1f0498942e2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439810"
---
# <a name="the-server-cleanup-wizard"></a>伺服器清理精靈

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

伺服器清理精靈已整合到使用者介面，而且可用來協助您管理您的磁碟空間。 此精靈可以執行下列作業：

- 移除未使用的更新，然後更新修訂中移除所有較舊的更新和未核准的更新修訂。

- 無法連絡伺服器刪除的電腦中刪除所有未在 30 天或多個連絡伺服器的用戶端電腦。

- 刪除所有更新的檔案不需要更新或下游伺服器的不必要的更新檔案刪除。

- 拒絕過期的更新拒絕所有 microsoft 已經過期的更新。

- 拒絕被取代的更新會拒絕所有符合下列所有準則的更新：

  -   並不一定要被取代的更新

  -   被取代的更新已在伺服器上的 30 天以上

  -   被取代的更新不目前報告所需的任何用戶端

  -   被取代的更新尚未明確地部署至電腦群組的 90 天以上

  -   必須核准取代的更新安裝至電腦群組

  > [!WARNING]
  >  在 WSUS 階層中，強烈建議您在第一次，最較低的下游/複本 WSUS 伺服器上執行清理程序，然後在階層中向上移動。 不正確地執行下游的每部伺服器上執行清除之前的任何上游伺服器上的 清除，可能會導致上游的資料庫中存在的資料與下游的資料庫之間不相符。 資料不符合上游和下游伺服器之間可能會導致同步處理失敗。 
  > 
  > [!IMPORTANT]
  >  如果您可以使用 [伺服器清理精靈]，以移除不必要的內容，也會移除您已從 Microsoft Update 類別目錄網站下載的所有私用的更新檔案。 您必須重新匯入這些檔案之後您執行 [伺服器清理精靈]。 

如果有更新核准使用自動核准規則，仍可能處於 「 已核准 」 狀態，而 伺服器清理精靈將不會移除。 若要移除處於 已核准 狀態的自動核准更新，WSUS 系統管理員必須-最小值-手動設定 「 未核准 」 的被取代更新的核准狀態以便將它們適合 declination 由 伺服器清理精靈。 伺服器清理精靈，可確保較新的更新已通過核准並沒有用戶端系統仍會報告視需要更新，再將標示為 [已拒絕]。 更新




