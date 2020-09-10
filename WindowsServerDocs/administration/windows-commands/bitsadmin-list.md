---
title: bitsadmin list
description: Bitsadmin list 命令的參考文章，其中列出目前使用者擁有的傳送作業。
ms.topic: reference
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 81fecf15f16cfa28933b63f9de693ba4e07679e8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631499"
---
# <a name="bitsadmin-list"></a>bitsadmin list

列出目前使用者擁有的傳送作業。

## <a name="syntax"></a>語法

```
bitsadmin /list [/allusers][/verbose]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| /allusers | 選擇性。 列出所有使用者的作業。 您必須具有系統管理員許可權，才能使用此參數。 |
| /verbose | 選擇性。 提供每項作業的詳細資訊。 |

## <a name="examples"></a>範例

取得目前使用者所擁有之作業的相關資訊。

```
bitsadmin /list
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
