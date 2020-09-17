---
title: 啟用為虛擬機器設定的所有虛擬網路介面卡
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
ms.date: 8/16/2016
ms.openlocfilehash: 81e0bbb8fce32b8343a13dea953498efb627d496
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746283"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>啟用為虛擬機器設定的所有虛擬網路介面卡

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*一或多個網路介面卡可能會在虛擬機器中停用。*

## <a name="impact"></a>影響

*下列虛擬機器可能沒有網路連線能力：*

\<list of virtual machine names>

## <a name="resolution"></a>解決方案

*使用客體作業系統中的裝置管理員來啟用所有虛擬網路介面卡。如果不需要介面卡，請使用 Hyper-v 管理員將它從虛擬機器中移除。*



