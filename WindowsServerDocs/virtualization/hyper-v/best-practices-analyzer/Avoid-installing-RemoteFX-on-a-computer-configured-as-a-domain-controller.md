---
title: 避免在設定為 Active Directory 網域控制站的電腦上安裝 RemoteFX
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
ms.date: 8/16/2016
ms.openlocfilehash: 1f639998c568035d0c992403cd0b1056ea4607b5
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747053"
---
# <a name="avoid-installing-remotefx-on-a-computer-that-is-configured-as-an-active-directory-domain-controller"></a>避免在設定為 Active Directory 網域控制站的電腦上安裝 RemoteFX

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*RemoteFX 是安裝在網域控制站上。*

## <a name="impact"></a>**影響**
*針對 RemoteFX 設定的虛擬電腦無法在這些電腦上使用。*

## <a name="resolution"></a>**解決方法**
*決定是否要使用 RemoteFX （Hyper-v）或 Active Directory 網網域控制站來設定此伺服器，然後視需要重新設定伺服器。*



