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
ms.openlocfilehash: 80eef106452a45ac4f071446ec8d427b757c443d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817019"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

傳回適合用來剖析資料。

## <a name="syntax"></a>語法

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>備註

帶狀線的新行字元，格式化輸出。

一般而言，您使用此命令搭配**建立**並**取得\*** 接收值的參數。 您必須指定此參數之前其他參數。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為工作狀態的未經處理資料*myDownloadJob*。
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)