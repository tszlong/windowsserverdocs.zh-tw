---
title: 這部伺服器上一或多部虛擬機器的複寫已暫停
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
ms.date: 8/16/2016
ms.openlocfilehash: 3a2bf07e1f93aed3966dd98ce608af53168bdf9e
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746513"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>這部伺服器上一或多部虛擬機器的複寫已暫停

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*一或多部虛擬機器的複寫已暫停。當主要虛擬機器暫停時，所發生的任何變更都會累積，並會在複寫繼續之後傳送至複本虛擬機器。*

## <a name="impact"></a>影響
*只要複寫暫停，主要虛擬機器中發生的累積變更將會耗用主伺服器上的可用磁碟空間。繼續複寫之後，可能會有大量的網路流量高載至複本伺服器。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*確認正在暫停複寫。如果複寫已暫停，以解決磁碟空間不足或網路連線能力，請在這些問題解決後立即繼續複寫。*



