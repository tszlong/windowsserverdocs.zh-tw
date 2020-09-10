---
title: 設定內容
description: Set coNtext 的參考文章，可設定陰影複製的建立內容。
ms.topic: reference
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7c89c0da8b63cef5b534163c5dbc1a0bee0cddbf
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637748"
---
# <a name="set-contex"></a>設定操作

設定用於建立陰影複製的內容。 如果使用時不含參數，請在命令提示字元中 **設定 coNtext** 顯示說明。



## <a name="syntax"></a>語法

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|clientaccessible|指定陰影複製可供用戶端版本的 Windows 使用。|
|持續|指定在程式結束、重設或重新開機期間保存陰影複製。|
|volatile|在結束或重設時刪除陰影複製。|
|nowriters|指定排除所有的寫入器。|

## <a name="remarks"></a>備註

-   *Clientaccessible*內容預設為持續性。

## <a name="examples"></a>範例

若要防止在您結束 DiskShadow 時刪除陰影複製，請輸入：
```
set context persistent
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)