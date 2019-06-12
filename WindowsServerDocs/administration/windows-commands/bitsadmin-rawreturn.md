---
title: bitsadmin rawreturn
description: 適用於 Windows 命令主題**bitsadmin rawreturn** -適用於剖析會傳回資料。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e12c8e621021d35ac618b4592515fe38c36be0e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434897"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

傳回適合用來剖析資料。

## <a name="syntax"></a>語法

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>備註

帶狀線的新行字元，格式化輸出。

一般而言，您使用此命令搭配**建立**並**取得\\** * 參數，以接收使用的值。 您必須指定此參數之前其他參數。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為工作狀態的未經處理資料*myDownloadJob*。
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)