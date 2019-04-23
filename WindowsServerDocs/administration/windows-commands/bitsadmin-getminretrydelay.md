---
title: bitsadmin getminretrydelay
description: 適用於 Windows 命令主題**bitsadmin getminretrydelay** -擷取時間 （秒），在發生暫時性錯誤，然後再嘗試將檔案傳輸後，等待服務的長度。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2a6df9faab8340994ad9219a863ad8e50186ccd1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832199"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay



擷取以秒為單位，在發生暫時性錯誤，然後再嘗試將檔案傳輸之後，服務會等待的時間長度。

## <a name="syntax"></a>語法

```
bitsadmin /GetMinRetryDelay <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的最小的重試延遲*myDownloadJob*。
```
C:\>bitsadmin /GetMinRetryDelay myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)