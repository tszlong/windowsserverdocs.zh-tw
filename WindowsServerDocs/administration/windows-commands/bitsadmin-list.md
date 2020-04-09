---
title: bitsadmin list
description: '**Bitsadmin 清單**的 Windows 命令主題，其中列出目前使用者擁有的傳送作業。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1883da7bfa71a41952f6f67e25eca4dbbdd3353c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850321"
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

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取目前使用者所擁有之作業的相關資訊。

```
C:\>bitsadmin /list
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)