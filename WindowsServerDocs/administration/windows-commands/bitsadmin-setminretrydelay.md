---
title: bitsadmin setminretrydelay
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 640492cf690a934e3e3b8d0ecf8ca7a0d6a7dc2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813079"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

設定最小的時間長度，以秒為單位，BITS 會等待在發生暫時性錯誤，然後再嘗試將檔案傳輸後。

## <a name="syntax"></a>語法

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|RetryDelay|表示以秒為單位的數字。|

## <a name="BKMK_examples"></a>範例

下列範例會設定名為作業的最小的重試延遲*myDownloadJob*為 35 秒。
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)