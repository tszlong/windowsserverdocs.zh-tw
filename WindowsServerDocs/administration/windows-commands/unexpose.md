---
title: unexpose
description: Diskshadow.exe 取消公開的參考文章，其 unexposes 使用公開命令公開的陰影複製。
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c315639746db84d49afd72fc2be89e757c8fb95
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87897070"
---
# <a name="unexpose"></a>unexpose

Unexposes 使用**公開**命令公開的陰影複製。 公開的陰影複製可以透過其陰影識別碼、磁碟機號、共用或掛接點來指定。



## <a name="syntax"></a>語法

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ShadowID>|Unexposes 給定陰影識別碼所指定的陰影複製。|
|\<Drive:>|Unexposes 與指定磁碟機號相關聯的陰影複製 (例如，磁片磁碟機 P) 。|
|\<Share>|Unexposes 與指定的共用相關聯的陰影複製 (例如 \\ \\ *MachineName* \) 。|
|\<MountPoint>|Unexposes 與指定掛接點相關聯的陰影複製 (例如，C:\shadowcopy \) 。|

## <a name="remarks"></a>備註

-   您可以使用現有的別名或環境變數來取代*ShadowID*。 請使用不含參數的**add**來查看現有的別名。

## <a name="examples"></a>範例

若要 diskshadow.exe 取消公開與 Drive P 相關聯的陰影複製，請輸入：
```
unexpose P:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)