---
title: retain
description: '* * * * 的參考文章'
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a0205f3b67bd99ca590c7ffc6fbd04b0eefd94f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883644"
---
# <a name="retain"></a>retain



準備要當做開機或系統磁碟區使用的現有動態簡單磁片區。

## <a name="syntax"></a>語法

```
retain
```

## <a name="remarks"></a>備註

-   在主開機記錄 (MBR) 動態磁碟上，此命令會在主開機記錄中建立磁碟分割專案。
-   在 GUID 磁碟分割表格上 (GPT) 動態磁碟，此命令會在 GUID 磁碟分割表格中建立磁碟分割專案。

## <a name="additional-references"></a>其他參考資料

