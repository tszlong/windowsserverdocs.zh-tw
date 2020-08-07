---
title: bitsadmin list
description: Bitsadmin list 命令的參考文章，其中列出目前使用者擁有的傳送作業。
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b4bf346cd52e09d81bfbb934df86b94b9b544aa
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893706"
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
| /verbose | 選擇性。 提供有關每項作業的詳細資訊。 |

## <a name="examples"></a>範例

取得目前使用者所擁有之作業的相關資訊。

```
bitsadmin /list
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
