---
title: 避免在系統磁片上儲存智慧分頁檔案
description: 瞭解當一或多部虛擬機器的記憶體設定可能需要在虛擬機器重新開機時使用智慧分頁，以及智慧分頁檔的指定位置是執行 Hyper-v 之伺服器的系統磁片時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
ms.date: 8/16/2016
ms.openlocfilehash: 990067509f0fbfe9ba19781da23ea3ea0b6e4091
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834593"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>避免在系統磁片上儲存智慧分頁檔案

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的文字。

## <a name="issue"></a>問題
*如果虛擬機器已重新開機，且智慧分頁檔案的指定位置是執行 Hyper-v 之伺服器的系統磁片，則一或多部虛擬機器的記憶體設定可能需要使用智慧分頁。*

## <a name="impact"></a>影響
*使用系統磁片進行智慧型分頁可能會導致執行 Hyper-v 的伺服器發生問題。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*重新設定虛擬機器，以將智慧分頁檔儲存在非系統磁片上。*



