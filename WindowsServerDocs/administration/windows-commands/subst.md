---
title: subst
description: 了解如何關聯的磁碟機代號的路徑。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 858195de89ca8661cf47c25b6cf9b519cc4efbf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858069"
---
# <a name="subst"></a>subst



關聯的磁碟機代號路徑。 如果未指定參數，使用**subst**作用中顯示的虛擬磁碟機名稱。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive1 >:|指定您要指派路徑的虛擬磁碟機。|
|[\<Drive2>:]\<Path>|指定您想要指派給虛擬磁碟機的路徑與實體磁碟機。|
|/d|刪除替代的 （虛擬） 磁碟機。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   下列命令無法運作，而且不應在中所指定的磁碟機上**subst**命令：

    **chkdsk**

    **diskcomp**

    **diskcopy**

    **格式**

    **label**

    **recover**
-   *Drive1*參數必須是所指定的範圍內**config**命令。 否則，請**subst**會顯示下列錯誤訊息：

    `Invalid parameter - drive1:`

## <a name="BKMK_examples"></a>範例

若要建立虛擬磁碟機 Z B:\User\Betty\Forms 的路徑，請輸入：
```
subst z: b:\user\betty\forms 
```
而不是輸入完整路徑，您都可以輸入後面接著冒號，如下所示的虛擬磁碟機代號來連線到這個目錄：
```
z: 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)