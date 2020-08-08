---
title: 針對 SR-IOV 設定的執行中虛擬機器數目不應超過虛擬機器可用的虛擬函式數目
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 16126118cdbf2341059fb0871ee7c33b62dc6b94
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960381"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>針對 SR-IOV 設定的執行中虛擬機器數目不應超過虛擬機器可用的虛擬函式數目

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題
*針對單一根目錄 i/o 虛擬化所設定的執行中虛擬機器數目，沒有足夠的可用虛擬函式 (SR-IOV) 。*

## <a name="impact"></a>影響
*下列虛擬機器的網路效能可能不佳：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*請考慮在不需要 SR-IOV 虛擬函式的一或多部虛擬機器上停用 SR-IOV。*



