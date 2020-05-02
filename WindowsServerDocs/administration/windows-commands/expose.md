---
title: 向
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 500dc5cfcd5e2bba4cfbc3cb5ef81a9065ea53cf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725672"
---
# <a name="expose"></a>向



將持續性陰影複製公開為磁碟機號、共用或掛接點。



## <a name="syntax"></a>語法

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|ShadowID|指定您要公開之陰影複製的陰影識別碼。|
|\<磁片磁碟機： >|將指定的陰影複製公開為磁碟機號（例如 P：）。|
|\<共用>|在\\ \\共用（例如*MachineName*\)）公開指定的陰影複製。|
|\<掛接點>|將指定的陰影複製公開到掛接點（例如，C:\shadowcopy\)。|

## <a name="remarks"></a>備註

-   您可以使用現有的別名或環境變數來取代*ShadowID*。 請使用不含參數的**add**來查看現有的別名。

## <a name="examples"></a>範例

若要將與 VSS_SHADOW_1 環境變數相關聯的持續陰影複製公開為磁片磁碟機 X，請輸入：
```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)