---
title: list providers
description: 列出提供者命令的參考文章，其中列出目前在系統上註冊的陰影複製提供者。
ms.topic: reference
ms.assetid: 844b4036-c0b9-449d-8347-7d58ef9bf16d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cab1277a79f71268b5e82702b39887b541918907
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028176"
---
# <a name="list-providers"></a>list providers

列出目前已在系統上註冊的陰影複製提供者。

## <a name="syntax"></a>語法

```
list providers
```

### <a name="examples"></a>範例

若要列出目前註冊的陰影複製提供者，請輸入：

```
list providers
```

類似下列顯示的輸出：

```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)