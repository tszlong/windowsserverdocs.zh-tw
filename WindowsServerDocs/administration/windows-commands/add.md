---
title: add
description: Add 命令的參考文章，會將磁片區新增至要陰影複製的磁片區集，或將別名新增至別名環境。
ms.topic: reference
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ddd090323b45caa0cc7afa4c07c568793f2e268d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633499"
---
# <a name="add"></a>add

將磁片區新增至要陰影複製的磁片區集合，或將別名新增至別名環境。 如果未使用子命令， **add** 會列出目前的磁片區和別名。

> [!NOTE]
> 在建立陰影複製之前，不會將別名新增至別名環境。 您需要立即使用 [ **新增別名**] 來新增您需要的別名。

## <a name="syntax"></a>語法

```
add
add volume <volume> [provider <providerid>]
add alias <aliasname> <aliasvalue>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| 磁碟區 | 將磁片區新增至陰影複製集，這是要陰影複製的磁片區集合。 請參閱 [加入磁片](add-volume.md) 區以取得語法和參數。 |
| alias | 將指定的名稱和值新增至別名環境。 請參閱加入語法和參數的 [別名](add-alias.md) 。 |
| /? | 在命令列顯示說明。 |

## <a name="examples"></a>範例

若要顯示已新增的磁片區，以及目前在環境中的別名，請輸入：

```
add
```

下列輸出顯示已將磁片磁碟機 C 新增至陰影複製集：

```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)