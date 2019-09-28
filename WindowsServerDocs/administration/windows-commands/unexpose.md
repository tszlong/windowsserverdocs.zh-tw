---
title: diskshadow.exe 取消公開
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e10126739ef82b060e271e9b804a77658b5ec82
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392261"
---
# <a name="unexpose"></a>diskshadow.exe 取消公開



unexposes 使用**公開**命令公開的陰影複製。 公開的陰影複製可以透過其陰影識別碼、磁碟機號、共用或掛接點來指定。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ShadowID >|Unexposes 給定陰影識別碼所指定的陰影複製。|
|\<Drive： >|Unexposes 與指定磁碟機號相關聯的陰影複製（例如，磁片磁碟機 P）。|
|\<Share >|Unexposes 與指定共用相關聯的陰影複製（例如，\\ @ no__t-1*MachineName*\)。|
|\<MountPoint >|Unexposes 與指定掛接點相關聯的陰影複製（例如，C:\shadowcopy @ no__t-0。|

## <a name="remarks"></a>備註

-   您可以使用現有的別名或環境變數來取代*ShadowID*。 請使用不含參數的**add**來查看現有的別名。

## <a name="BKMK_examples"></a>典型

若要 diskshadow.exe 取消公開與 Drive P 相關聯的陰影複製，請輸入：
```
unexpose P:
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)