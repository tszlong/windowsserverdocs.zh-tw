---
title: unexpose
description: Diskshadow.exe 取消公開的參考文章，unexposes 使用公開命令公開的陰影複製。
ms.topic: reference
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13e14941e2c67aa0361dcc0af2cdb1a36bf7e651
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036406"
---
# <a name="unexpose"></a>unexpose

Unexposes 使用 **公開** 命令公開的陰影複製。 公開的陰影複製可透過其陰影識別碼、磁碟機號、共用或掛接點來指定。



## <a name="syntax"></a>語法

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ShadowID>|Unexposes 由指定的陰影識別碼所指定的陰影複製。|
|\<Drive:>|Unexposes 與指定之磁碟機號相關聯的陰影複製 (例如，磁片磁碟機 P) 。|
|\<Share>|Unexposes 與指定共用相關聯的陰影複製 (例如 \\ \\ *MachineName* \) 。|
|\<MountPoint>|Unexposes 與指定掛接點相關聯的陰影複製 (例如，C:\shadowcopy \) 。|

## <a name="remarks"></a>備註

-   您可以使用現有的別名或環境變數來取代 *ShadowID*。 使用不含參數的 **add** 來查看現有的別名。

## <a name="examples"></a>範例

若要 diskshadow.exe 取消公開與磁片磁碟機 P 相關聯的陰影複製，請輸入：
```
unexpose P:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)