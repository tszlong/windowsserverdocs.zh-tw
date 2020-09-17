---
title: 必須先安裝 Integration services，主要或複本虛擬機器才能在容錯移轉之後使用替代 IP 位址
description: 此最佳做法分析程式規則之文字的線上版本，並提供詳細資訊的連結。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
ms.date: 8/16/2016
ms.openlocfilehash: 3a612c9e119ac2b74bea070feb458703dd50040f
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745823"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>必須先安裝 Integration services，主要或複本虛擬機器才能在容錯移轉之後使用替代 IP 位址

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*參與複寫的虛擬機器可以設定為在容錯移轉時使用特定的 IP 位址，但僅限於在虛擬機器的客體作業系統中安裝 integration services 時。*

## <a name="impact"></a>影響
*當容錯移轉 (已規劃、未規劃或測試) 時，複本虛擬機器將會使用與主要虛擬機器相同的 IP 位址來連線。此設定可能會導致連線問題。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*使用虛擬機器連線在虛擬機器中安裝 integration services。*

從 Windows Server 2016，適用于 Windows 虛擬機器的 integration services 會透過 Windows Update 傳遞。 請確定這些虛擬機器已設定為接收 Windows 更新，以取得最新版本的 integration services。 Linux 核心現在包含 Linux integration services (的 .LIS) 並已針對新版本更新，但以較舊的核心為基礎的 Linux 發行版本可能沒有最新的增強功能或修正。 如需詳細資訊，請參閱 [Windows 上適用于 hyper-v 的支援 Linux 和 FreeBSD 虛擬機器](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。


