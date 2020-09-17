---
title: 當虛擬機器設定為使用 SR-IOV 時，請確定虛擬函式驅動程式能正常運作
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
ms.date: 8/16/2016
ms.openlocfilehash: 3be2f616ab8981e887688404f2907524f5f53e49
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746843"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>當虛擬機器設定為使用 SR-IOV 時，請確定虛擬函式驅動程式能正常運作

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*虛擬函式驅動程式未在一或多部虛擬機器的客體作業系統中正確運作。*

## <a name="impact"></a>影響
*下列虛擬機器的網路效能不佳：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*在客體作業系統中，執行下列動作：確認已安裝適當的驅動程式，並已啟用所有網路裝置，並檢查事件記錄檔中的錯誤或警告。*



