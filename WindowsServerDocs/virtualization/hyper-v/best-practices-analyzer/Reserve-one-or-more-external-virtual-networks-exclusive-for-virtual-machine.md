---
title: 保留一或多個外部虛擬網路供虛擬機器獨佔使用
description: 提供指示以解決這個最佳做法分析程式規則所報告的問題。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
ms.date: 8/16/2016
ms.openlocfilehash: 8039f5ef94f1ca762a994607d5a1faf8eac0d98e
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746503"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>保留一或多個外部虛擬網路供虛擬機器獨佔使用

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*所有外部虛擬網路都設定為可供管理作業系統和虛擬機器使用。*

## <a name="impact"></a>影響

*管理作業系統中的網路效能可能會降低。*

## <a name="resolution"></a>解決方案

*使用虛擬交換器管理員來停止與管理作業系統共用外部虛擬網路。*

#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>停止與管理作業系統共用外部虛擬網路

1.  開啟 Hyper-V 管理員。 按一下 [開始]****，指向 [系統管理工具]****，然後按一下 [Hyper-V 管理員]****。

2.  從 [動作]**** 功能表上，按一下 [虛擬交換器管理員]****。

3.  在 [ **虛擬交換器**] 底下，按一下外部虛擬交換器的名稱。

4.  在 [ **連線類型** ] 區域中，于實體網路介面卡的名稱底下，清除 [ **允許管理作業系統共用此網路介面卡** ] 核取方塊。

5.  按一下 [確定]  。



