---
title: bitsadmin setminretrydelay
description: 適用于 bitsadmin setminretrydelay 的 Windows 命令主題，會設定在嘗試傳輸檔案之前，在遇到暫時性錯誤之後，BITS 等待的最短時間長度（以秒為單位）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb2fe4c6d0e4f90c6ec49fa1da63404393d4f634
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849361"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

設定在嘗試傳輸檔案之前，在遇到暫時性錯誤之後，BITS 等待的最短時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|RetryDelay|以秒數表示的數位。|

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會將名為*myDownloadJob*之作業的最小重試延遲設定為35秒。
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)