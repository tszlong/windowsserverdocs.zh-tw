---
title: bitsadmin getreplydata
description: 適用於 Windows 命令主題**bitsadmin getreplydata** -擷取伺服器的回覆資料，以十六進位格式。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78d70a44d6881568c8d92db145fdf22a260ee8af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883819"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

擷取伺服器的回覆資料，以十六進位格式。

**1.2 及更早版本的位元**: 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /GetReplyData <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

僅適用於上傳-回覆作業。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的回覆資料*myDownloadJob*。
```
C:\>bitsadmin /GetReplyData myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)