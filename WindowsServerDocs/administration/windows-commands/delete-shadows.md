---
title: 刪除陰影
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a0945477bc4fce907b5ec4a697c7a2ec2f59557
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436115"
---
# <a name="delete-shadows"></a>刪除陰影



刪除陰影複製。

## <a name="syntax"></a>語法

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

## <a name="parameters"></a>參數

|     參數     |                                                                             描述                                                                              |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        全部        |                                                                      刪除所有的陰影複製。                                                                      |
| 磁碟區\<磁碟區 >  |                                                            刪除所有指定的磁碟區陰影複製。                                                            |
| 最舊\<磁碟區 >  |                                                         刪除指定的磁碟區的最舊的陰影複製。                                                          |
|   set \<SetID>    | 刪除在指定 ID 的陰影複本集中的陰影複製 您可以使用，以便指定別名 **%** 符號如果別名存在於目前的環境。 |
|  識別碼\<ShadowID >   |              刪除指定 ID 的陰影複製 您可以使用，以便指定別名 **%** 符號如果別名存在於目前的環境。               |
| 公開 {\<磁碟機 > |                                                                            <MountPoint>}                                                                             |

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)