---
title: add
description: Add 命令的參考文章，其會將磁片區新增至要陰影複製的磁片區集合，或將別名新增至別名環境。
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8ba5f425617fe48d10a900c82fbfcf9c174214f5
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895587"
---
# <a name="add"></a>add

將磁片區新增至要陰影複製的磁片區集合，或將別名新增至別名環境。 如果使用時不含子命令， **add**會列出目前的磁片區和別名。

> [!NOTE]
> 在建立陰影複製之前，別名不會新增至別名環境。 您應該立即使用 [新增**別名**] 來新增您需要的別名。

## <a name="syntax"></a>語法

```
add
add volume <volume> [provider <providerid>]
add alias <aliasname> <aliasvalue>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| 磁碟區 | 將磁片區新增至陰影複製組，這是要陰影複製的一組磁片區。 如需語法和參數，請參閱[新增磁片](add-volume.md)區。 |
| alias | 將指定的名稱和值加入至別名環境。 如需語法和參數，請參閱[add alias](add-alias.md) 。 |
| /? | 在命令列中顯示說明。 |

## <a name="examples"></a>範例

若要顯示新增的磁片區和目前在環境中的別名，請輸入：

```
add
```

下列輸出顯示磁片磁碟機 C 已新增至陰影複製組：

```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)