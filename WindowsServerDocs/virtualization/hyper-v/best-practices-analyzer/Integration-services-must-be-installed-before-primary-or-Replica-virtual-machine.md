---
title: 必須先安裝 Integration services，主要或複本虛擬機器才能在容錯移轉之後使用替代的 IP 位址
description: 此最佳做法分析程式規則的線上版本文字，包含詳細資訊的連結。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 171b9b4a41f012be3262dbddea0527381282d105
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946011"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>必須先安裝 Integration services，主要或複本虛擬機器才能在容錯移轉之後使用替代的 IP 位址

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
*參與複寫的虛擬機器可以設定為在容錯移轉時使用特定的 IP 位址，但只有在虛擬機器的客體作業系統中已安裝整合服務時。*

## <a name="impact"></a>影響
*當容錯移轉 (規劃、未計畫或測試) 時，複本虛擬機器將會使用與主要虛擬機器相同的 IP 位址上線。此設定可能會導致連線問題。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*使用虛擬機器連線，在虛擬機器中安裝整合服務。*

從 Windows Server 2016，Windows 虛擬機器的 integration services 會透過 Windows Update 傳遞。 請確定這些虛擬機器已設定為接收 Windows 更新，以取得最新版本的 integration services。 Linux 核心現在包含 Linux integration services (.LIS) 並已針對新版本更新，但以較舊核心為基礎的 Linux 發行版本可能沒有最新的增強功能或修正。 如需詳細資訊，請參閱[Windows 上的 Hyper-v 支援的 Linux 和 FreeBSD 虛擬機器](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。


