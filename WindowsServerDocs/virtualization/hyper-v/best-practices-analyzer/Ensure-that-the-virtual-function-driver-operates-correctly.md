---
title: 當虛擬機器設定為使用 SR-IOV 時，請確定虛擬函式驅動程式能正常運作
description: 瞭解當虛擬函式驅動程式未在一或多部虛擬機器的客體作業系統中正確運作時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
ms.date: 8/16/2016
ms.openlocfilehash: a009060dc67ba29c12e986167b2ed746865311d5
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845920"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>當虛擬機器設定為使用 SR-IOV 時，請確定虛擬函式驅動程式能正常運作

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
*虛擬函式驅動程式未在一或多部虛擬機器的客體作業系統中正確運作。*

## <a name="impact"></a>影響
*下列虛擬機器的網路效能不佳：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*在客體作業系統中，執行下列動作：確認已安裝適當的驅動程式，並已啟用所有網路裝置，並檢查事件記錄檔中的錯誤或警告。*



