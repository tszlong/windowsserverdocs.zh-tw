---
title: 虛擬機器至少應每週備份一次
description: 瞭解當一或多部虛擬機器未在過去一周內備份時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
ms.date: 8/16/2016
ms.openlocfilehash: 9c2cc7836eca57b5d7ae7e180a8af5c6e3104996
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846299"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>虛擬機器至少應每週備份一次

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*過去一周內尚未備份一或多部虛擬機器。*

## <a name="impact"></a>影響
*如果虛擬機器遇到問題且最近的備份不存在，可能會發生嚴重的資料遺失。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*排程虛擬機器的備份至少一周執行一次。如果此虛擬機器是複本且其主要虛擬機器正在進行備份，或者這是主要虛擬機器及其複本正在進行備份，您可以忽略這個規則。*



