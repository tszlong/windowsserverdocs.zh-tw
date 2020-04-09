---
title: 新增
description: 適用于**add**的 Windows 命令主題，其會將磁片區新增至要陰影複製的磁片區集合，或將別名新增至別名環境。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9895082cc10223fd08cff6916c20c3af5613e947
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851341"
---
# <a name="add"></a>新增

將磁片區新增至要陰影複製的磁片區集合，或將別名新增至別名環境。 如果使用時不含子命令， **add**會列出目前的磁片區和別名。

> [!NOTE]
> 在建立陰影複製之前，別名不會新增至別名環境。 您應該立即使用 [新增**別名**] 來新增您需要的別名。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
add 
add volume <Volume> [provider <ProviderID>] 
add alias <AliasName> <AliasValue>
```

## <a name="add-subcommands"></a>新增子命令

| 來 | 描述 |
| ---------- | ----------- |
| 磁碟區 | 將磁片區新增至陰影複製組，這是要陰影複製的一組磁片區。 如需語法和參數，請參閱[新增磁片](add-volume.md)區。 |
| Alias - 別名 | 將指定的名稱和值加入至別名環境。 如需語法和參數，請參閱[Add alias](add-alias.md) 。 |
| `/?` | 在命令列中顯示說明。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

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