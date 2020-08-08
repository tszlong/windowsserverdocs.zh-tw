---
title: 應該將複寫的重新同步處理排程為離峰時段
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: dad983e19df328946f3bd7ec59f68ee3e9f8bfcc
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939004"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>應該將複寫的重新同步處理排程為離峰時段

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
*主要虛擬機器的複寫重新同步處理未排定在離峰時間執行。*

## <a name="impact"></a>影響
*虛擬機器處於需要重新同步處理的狀態愈久，複寫記錄檔的成長越長，主要虛擬機器上發生的變更也越多。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*使用 [Hyper-v 管理員] 修改虛擬機器的複寫設定，以在離峰時段自動執行重新同步處理。*



