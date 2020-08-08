---
title: 設定以 VSS 為基礎的備份的客體作業系統，為 Hyper-v 複本啟用應用程式一致快照
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 5270584b6213ad59ef43c378e5aa7a5dbcc30a4e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939102"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>設定以 VSS 為基礎的備份的客體作業系統，為 Hyper-v 複本啟用應用程式一致快照

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題
*應用程式一致快照集需要在參與複寫之虛擬機器的客體作業系統中啟用和設定「磁片區陰影複製服務 (VSS) 。*

## <a name="impact"></a>影響
*即使在複寫設定中指定了應用程式一致快照集，Hyper-v 也不會使用它們，除非已設定 VSS。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*使用虛擬機器連線，在虛擬機器中安裝整合服務。*



