---
title: 刪除陰影
description: 刪除陰影的參考主題，這會刪除陰影複製。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dd367d76ad1699321af9caf47a0ddc351088a05
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720803"
---
# <a name="delete-shadows"></a>刪除陰影

刪除陰影複製。

## <a name="syntax"></a>語法

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---- | ---- |
| all | 刪除所有陰影複製。 |
| 磁片\<區磁片區> | 刪除指定磁片區的所有陰影複製。 |
| 最\<舊的磁片區> | 刪除指定磁片區最舊的陰影複製。 |
| 設定\<SetID> | 刪除指定識別碼之陰影複製組中的陰影複製。 如果別名存在於目前的環境中**%** ，您可以使用符號來指定別名。 |
| 識別碼\<ShadowID> | 刪除指定識別碼的陰影複本。 如果別名存在於目前的環境中**%** ，您可以使用符號來指定別名。 |
| 已公開\<{磁片磁碟機> | <MountPoint>} |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)