---
title: bitsadmin getminretrydelay
description: '**Bitsadmin getminretrydelay**的 Windows 命令主題-抓取在嘗試傳輸檔案之前，服務在遇到暫時性錯誤之後所等待的時間長度（以秒為單位）。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a2bde6340034e48b97b4c86f48a3b2ef72560a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381546"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay



抓取在嘗試傳輸檔案之前，服務在遇到暫時性錯誤之後所等待的時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /GetMinRetryDelay <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的最小重試延遲。
```
C:\>bitsadmin /GetMinRetryDelay myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)