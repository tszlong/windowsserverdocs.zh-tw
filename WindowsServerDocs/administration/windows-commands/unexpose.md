---
title: 取消公開
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fe9cb5dfd8ae6c71fdc72ddc1e8421229f98f5d0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837469"
---
# <a name="unexpose"></a>取消公開



Unexposes 的陰影複製，使用已公開**公開**命令。 陰影識別碼、 磁碟機代號、 共用或掛接點可以指定公開的陰影複製。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ShadowID >|Unexposes 指定陰影 ID 所指定的陰影複製|
|\<磁碟機： >|Unexposes 與指定的磁碟機代號 （例如，為磁碟機 P） 相關聯的陰影複製。|
|\<Share>|Unexposes 與指定的共用相關聯的陰影複製 (例如\\ \\ *MachineName*\)。|
|\<MountPoint>|Unexposes 與指定的掛接點相關聯的陰影複製 (例如 C:\shadowcopy\)。|

## <a name="remarks"></a>備註

-   您可以使用現有的別名或環境變數的位置*ShadowID*。 使用**新增**不含參數，若要查看現有的別名。

## <a name="BKMK_examples"></a>範例

若要取消公開的磁碟機 P 與相關聯的陰影複製，請輸入：
```
unexpose P:
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)