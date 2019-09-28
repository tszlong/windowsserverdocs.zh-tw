---
title: bitsadmin addfileset
description: '**Bitsadmin addfileset**的 Windows 命令主題-將一或多個檔案新增至指定的作業。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 464f2da151d5a7bfffde286e52d9158560d48dcc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381987"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

將一或多個檔案加入至指定的作業。

## <a name="syntax"></a>語法

```
bitsadmin /addfileset <Job> <TextFile>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|TextFile|文字檔，每一行包含遠端和本機檔案名。</br>注意：名稱會以空格分隔。 以 # 字元開頭的行會被視為批註。|

## <a name="BKMK_examples"></a>典型

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)