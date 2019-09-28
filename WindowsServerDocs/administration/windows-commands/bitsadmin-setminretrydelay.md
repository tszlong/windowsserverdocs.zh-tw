---
title: bitsadmin setminretrydelay
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 379dfa8bfdc48969f268fd1c9544d3bee8bbe646
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380509"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

設定在嘗試傳輸檔案之前，在遇到暫時性錯誤之後，BITS 等待的最短時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|RetryDelay|以秒數表示的數位。|

## <a name="BKMK_examples"></a>典型

下列範例會將名為*myDownloadJob*之作業的最小重試延遲設定為35秒。
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)