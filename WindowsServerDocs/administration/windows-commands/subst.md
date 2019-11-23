---
title: subst
description: 瞭解如何建立路徑與磁碟機號的關聯。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f3010d1e58fbd360b8311512e6664873b020c12b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383751"
---
# <a name="subst"></a>subst



建立路徑與磁碟機號的關聯。 如果使用時不含參數， **subst**會顯示作用中虛擬磁片磁碟機的名稱。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|\<Drive1 >：|指定您要指派路徑的虛擬磁片磁碟機。|
|[\<Drive2 >：]\<路徑 >|指定您要指派給虛擬磁片磁碟機的實體磁片磁碟機和路徑。|
|/d|刪除替代的（虛擬）磁片磁碟機。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   下列命令無法使用，且不應用於**subst**命令中指定的磁片磁碟機：

    **chkdsk**

    **diskcomp**

    **diskcopy**

    **format**

    **label**

    **recover**
-   *Drive1*參數必須在**lastdrive**命令所指定的範圍內。 如果不是，則**subst**會顯示下列錯誤訊息：

    `Invalid parameter - drive1:`

## <a name="BKMK_examples"></a>典型

若要建立路徑 B:\User\Betty\Forms 的虛擬磁片磁碟機 Z，請輸入：
```
subst z: b:\user\betty\forms 
```
您可以輸入虛擬磁片磁碟機的字母，後面加上冒號，而不是輸入完整路徑，如下所示：
```
z: 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)