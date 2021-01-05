---
title: 這部伺服器上一或多部虛擬機器的複寫已暫停
description: 瞭解當一或多部虛擬機器的複寫暫停時，該怎麼辦。 當主要虛擬機器暫停時，所發生的任何變更都會累積，並會在複寫繼續之後傳送至複本虛擬機器。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
ms.date: 8/16/2016
ms.openlocfilehash: bc8a173773217ea00c47080ded74901f70384b2a
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846122"
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



