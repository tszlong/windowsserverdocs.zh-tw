---
title: 向
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55cc7a292b81977a346f3f078a3b5623243ea46c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844801"
---
# <a name="expose"></a>向



將持續性陰影複製公開為磁碟機號、共用或掛接點。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|ShadowID|指定您要公開之陰影複製的陰影識別碼。|
|\<磁片磁碟機： >|將指定的陰影複製公開為磁碟機號（例如 P：）。|
|\<共用 >|在共用上公開指定的陰影複製（例如，\\\\*MachineName*\)。|
|\<掛接點 >|將指定的陰影複製公開到掛接點（例如，C:\shadowcopy\)。|

## <a name="remarks"></a>備註

-   您可以使用現有的別名或環境變數來取代*ShadowID*。 請使用不含參數的**add**來查看現有的別名。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要將與 VSS_SHADOW_1 環境變數相關聯的持續陰影複製公開為磁片磁碟機 X，請輸入：
```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)