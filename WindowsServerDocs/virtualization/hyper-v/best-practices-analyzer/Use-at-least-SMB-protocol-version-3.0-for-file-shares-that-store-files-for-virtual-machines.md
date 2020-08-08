---
title: 針對儲存虛擬機器檔案的檔案共用，請至少使用 SMB 通訊協定版本3.0。
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 4bb832b8-f1aa-4c1f-a0f2-324dd53553ea
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: b2393e2aa0418758ff59c527cef6f38a0c8b8402
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948385"
---
# <a name="use-at-least-smb-protocol-version-30-for-file-shares-that-store-files-for-virtual-machines"></a>針對儲存虛擬機器檔案的檔案共用，請至少使用 SMB 通訊協定版本3.0。

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>**問題**
*虛擬機器檔案或虛擬硬碟檔案儲存在不支援 SMB 通訊協定3.0 版的檔案共用上。*

## <a name="impact"></a>**影響**
*Microsoft 不支援這種設定。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方案**
*將檔案移至至少使用 SMB 通訊協定3.0 版的檔案共用。*



