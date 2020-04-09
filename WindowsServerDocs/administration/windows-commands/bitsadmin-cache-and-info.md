---
title: bitsadmin 快取與資訊
description: Bitsadmin 快取**和資訊**的 Windows 命令主題，會傾印特定的快取專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e9c6ce1eb972a76408483b8a27a3abca5500e56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850891"
---
# <a name="bitsadmin-cache-and-info"></a>bitsadmin 快取與資訊

傾印特定的快取專案。

## <a name="syntax"></a>語法

```
bitsadmin /cache /info recordID [/verbose]
```

### <a name="parameters"></a>參數

| Paramreter | 描述 |
| -------------- | -------------- |
| recordID | 與快取專案相關聯的 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會傾印具有 {6511FB02-E195-40A2-B595-E8E2F8F47702} recordID 值的快取專案。

```
C:\>bitsadmin /cache /info {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)