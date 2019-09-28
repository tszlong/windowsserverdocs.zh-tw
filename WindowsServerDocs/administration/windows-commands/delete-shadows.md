---
title: 刪除陰影
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c965af8b045c5ab3a110542d148b255f382a95c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378634"
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
|        全部        |                                                                      刪除所有陰影複製。                                                                      |
| 磁片區 \<Volume >  |                                                            刪除指定磁片區的所有陰影複製。                                                            |
| 最舊的 \<Volume >  |                                                         刪除指定磁片區最舊的陰影複製。                                                          |
|   設定 \<SetID >    | 刪除指定識別碼之陰影複製組中的陰影複製。 如果別名存在於目前的環境中，您可以使用 **%** 符號來指定別名。 |
|  識別碼 \<ShadowID >   |              刪除指定識別碼的陰影複本。 如果別名存在於目前的環境中，您可以使用 **%** 符號來指定別名。               |
| 公開 {\<Drive > |                                                                            <MountPoint>}                                                                             |

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)