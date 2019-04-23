---
title: bitsadmin addfileset
description: 適用於 Windows 命令主題**bitsadmin addfileset** -將一或多個檔案新增至指定的作業。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f8f6ff32dfa6042272c68647477d77183ce9cb76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889439"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

將指定的作業中的一個或多個檔案。

## <a name="syntax"></a>語法

```
bitsadmin /addfileset <Job> <TextFile>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|TextFile|文字檔案，其中每一行都包含遠端和本機檔案名稱。</br>注意：名稱是以空格分隔。 行開頭為 # 字元會被視為註解。|

## <a name="BKMK_examples"></a>範例

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)