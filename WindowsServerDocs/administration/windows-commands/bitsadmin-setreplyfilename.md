---
title: bitsadmin setreplyfilename
description: 適用于 bitsadmin setreplyfilename 的 Windows 命令主題，其指定包含伺服器回復之檔案的路徑。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd45174a7deac89cc943fb19d544e372c0198139
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849181"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

指定包含伺服器回復之檔案的路徑。

**BITS 1.2 和更早版本**：不支援。

## <a name="syntax"></a>語法

```
bitsadmin /SetReplyFileName <Job> <Path>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|路徑|放置伺服器回復的位置|

## <a name="remarks"></a>備註

僅適用于上傳-回復作業。

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會針對名為*myDownloadJob*的作業設定回復檔案名 pathfor。
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)