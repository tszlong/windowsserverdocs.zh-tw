---
title: 設定您希望複本虛擬機器在容錯移轉時使用的容錯移轉 TCP/IP 設定
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 713be5cf428617287e0be0bc65b3e2beb2d11400
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948446"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>設定您希望複本虛擬機器在容錯移轉時使用的容錯移轉 TCP/IP 設定

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
*使用靜態 IP 位址設定的複本虛擬機器，應該設定為在容錯移轉時，使用其主要虛擬機器對應項的不同 IP 位址。*

## <a name="impact"></a>影響
*使用主要虛擬機器所支援之工作負載的用戶端，可能無法在容錯移轉之後連線到複本虛擬機器。此外，主要虛擬機器的原始 IP 位址在複本虛擬機器網路拓撲中將無效。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*使用 [Hyper-v 管理員] 來設定複本虛擬機器在容錯移轉時應使用的 IP 位址。*



