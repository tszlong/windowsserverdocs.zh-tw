---
title: bitsadmin cache 和 deleteURL
description: Bitsadmin cache 和 deleteURL 命令的參考主題，它會刪除指定 URL 的所有快取專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 075c48e5c8c205cbbf3fe476260ec7909edcc3e6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718451"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>bitsadmin cache 和 deleteURL

刪除指定 URL 的所有快取專案。

## <a name="syntax"></a>語法

```
bitsadmin /deleteURL URL
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| URL | 識別遠端檔案的統一資源定位器。 |

## <a name="examples"></a>範例

若要刪除的`https://www.contoso.com/en/us/default.aspx`所有快取專案：

```
bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx 
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin cache 命令](bitsadmin-cache.md)
