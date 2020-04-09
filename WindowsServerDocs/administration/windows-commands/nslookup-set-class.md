---
title: nslookup set class
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc3bb4e36582f01584c0b89a12d43874322c3190
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838581"
---
# <a name="nslookup-set-class"></a>nslookup set class



變更查詢類別。 類別會指定資訊的通訊協定群組。

## <a name="syntax"></a>語法

```
set class=<Class>
```

### <a name="parameters"></a>參數

| 參數 |                                                                                                                                    描述                                                                                                                                    |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<類別 >  | 預設類別是在中。 以下列出此命令的有效值。</br>-IN：指定網際網路類別。</br>-混亂：指定混亂類別。</br>-HESIOD：指定 MIT Athena-io Hesiod 類別。</br>-ANY：指定先前列出的任何萬用字元。 |
|   {說明   |                                                                                                                                        ?}                                                                                                                                         |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)