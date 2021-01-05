---
title: 啟用為虛擬機器設定的所有虛擬網路介面卡
description: 瞭解當虛擬機器中有一或多個網路介面卡可停用時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
ms.date: 8/16/2016
ms.openlocfilehash: 300aad537c5e111003db60b1ba947dbea447ada9
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846372"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>啟用為虛擬機器設定的所有虛擬網路介面卡

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*一或多個網路介面卡可能會在虛擬機器中停用。*

## <a name="impact"></a>影響

*下列虛擬機器可能沒有網路連線能力：*

\<list of virtual machine names>

## <a name="resolution"></a>解決方案

*使用客體作業系統中的裝置管理員來啟用所有虛擬網路介面卡。如果不需要介面卡，請使用 Hyper-v 管理員將它從虛擬機器中移除。*



