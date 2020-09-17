---
title: 針對以 VSS 為基礎的備份設定客體作業系統，以啟用 Hyper-v 複本的應用程式一致快照集
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
ms.date: 8/16/2016
ms.openlocfilehash: b6a7eec504282e63e0cb24efbd2cdc5f66849005
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746883"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>針對以 VSS 為基礎的備份設定客體作業系統，以啟用 Hyper-v 複本的應用程式一致快照集

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*應用程式一致快照集需要在參與複寫的虛擬機器的客體作業系統中啟用和設定磁片區陰影複製服務 (VSS) 。*

## <a name="impact"></a>影響
*即使在複寫設定中指定了應用程式一致快照集，除非已設定 VSS，否則 Hyper-v 不會使用它們。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*使用虛擬機器連線在虛擬機器中安裝 integration services。*



