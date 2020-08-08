---
title: 虛擬機器至少應備份每週一次
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: fccc0bb7c46206df210f699cdda84507343c342a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960201"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>虛擬機器至少應備份每週一次

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
*一部或多部虛擬機器在過去一周內尚未備份。*

## <a name="impact"></a>影響
*如果虛擬機器遇到問題，而且最近的備份不存在，可能會發生重大資料遺失的情況。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*將虛擬機器的備份排程為每週至少執行一次。如果此虛擬機器是複本，而且其主要虛擬機器正在進行備份，或如果這是主要虛擬機器，且其複本正在進行備份，您可以忽略此規則。*



