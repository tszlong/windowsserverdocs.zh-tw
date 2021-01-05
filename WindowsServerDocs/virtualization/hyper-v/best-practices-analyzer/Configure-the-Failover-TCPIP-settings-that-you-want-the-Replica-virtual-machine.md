---
title: 設定您希望複本虛擬機器在發生容錯移轉時使用的容錯移轉 TCP/IP 設定
description: 瞭解當使用靜態 IP 位址設定的複本虛擬機器應設定為在容錯移轉時，使用其主要虛擬機器對應的不同 IP 位址時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.date: 8/16/2016
ms.openlocfilehash: 2a52cdd937589476abe254ea923b376d0d3472ed
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846436"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>設定您希望複本虛擬機器在發生容錯移轉時使用的容錯移轉 TCP/IP 設定

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*使用靜態 IP 位址設定的複本虛擬機器，應設定為在容錯移轉時，使用與其主要虛擬機器對應的不同 IP 位址。*

## <a name="impact"></a>影響
*使用主要虛擬機器所支援之工作負載的用戶端，在容錯移轉之後可能無法連線到複本虛擬機器。此外，主要虛擬機器的原始 IP 位址在複本虛擬機器網路拓撲中將無效。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*使用 Hyper-v 管理員來設定複本虛擬機器在容錯移轉時應使用的 IP 位址。*



