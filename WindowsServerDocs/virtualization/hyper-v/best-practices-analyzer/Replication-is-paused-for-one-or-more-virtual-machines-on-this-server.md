---
title: 這部伺服器上的一或多部虛擬機器的複寫已暫停
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a40c4d45eea6d0c363cd03d5eef94543ddc1317d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948426"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>這部伺服器上的一或多部虛擬機器的複寫已暫停

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題
*一或多部虛擬機器的複寫已暫停。當主要虛擬機器暫停時，發生的任何變更都會累積，並在複寫繼續後傳送至複本虛擬機器。*

## <a name="impact"></a>影響
*只要暫停複寫，主要虛擬機器中發生的累積變更就會耗用主伺服器上的可用磁碟空間。繼續複寫之後，可能會有大量高載的網路流量傳送到複本伺服器。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*確認暫停複寫是預定的。如果複寫已暫停，以解決磁碟空間不足或網路連線的問題，請在這些問題解決後立即繼續複寫。*



