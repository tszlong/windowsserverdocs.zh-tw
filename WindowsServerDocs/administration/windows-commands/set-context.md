---
title: 設定內容
description: Set coNtext 的參考文章，可設定陰影複製建立的內容。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98fb69f84b15a2444d24e4b6515ff9ff665b9aa7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937108"
---
# <a name="set-contex"></a>設定操作

設定陰影複製建立的內容。 如果使用時不含參數， **set coNtext**會在命令提示字元中顯示 help。



## <a name="syntax"></a>語法

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|clientaccessible|指定陰影複製可供用戶端版本的 Windows 使用。|
|持續|指定陰影複製在程式結束、重設或重新開機時持續存在。|
|volatile|在結束或重設時刪除陰影複製。|
|nowriters|指定排除所有寫入器。|

## <a name="remarks"></a>備註

-   根據預設， *clientaccessible*內容是持續性的。

## <a name="examples"></a>範例

若要防止在您結束 DiskShadow 時刪除陰影複製，請輸入：
```
set context persistent
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)