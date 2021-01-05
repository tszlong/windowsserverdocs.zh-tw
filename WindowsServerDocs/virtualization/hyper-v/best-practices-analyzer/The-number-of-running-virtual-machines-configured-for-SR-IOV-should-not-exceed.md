---
title: 針對 SR-IOV 設定的執行中虛擬機器數目不應超過虛擬機器可用的虛擬功能數目
description: 瞭解沒有足夠的虛擬函式可用來設定用於單一根目錄 i/o 虛擬化 (SR-IOV) 的虛擬機器數目時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
ms.date: 8/16/2016
ms.openlocfilehash: 783b5dbf170756e922b448d73881543b5a811ce9
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845892"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>針對 SR-IOV 設定的執行中虛擬機器數目不應超過虛擬機器可用的虛擬功能數目

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
*沒有足夠的虛擬函式可用來執行針對單一根目錄 i/o 虛擬化 (SR-IOV) 所設定的執行中虛擬機器數目。*

## <a name="impact"></a>影響
*下列虛擬機器的網路效能可能不佳：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*請考慮在不需要 SR-IOV 虛擬功能的一或多部虛擬機器上停用 SR-IOV。*



