---
title: add alias
description: 新增別名命令的參考文章，此命令會將別名新增至別名環境。
ms.topic: reference
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1cc9b7c5395c29ac917a0b25b97fb091dc66a699
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633557"
---
# <a name="add-alias"></a>add alias

將別名新增至別名環境。 如果使用時不含任何參數，請在命令提示字元中 **加入別名** 顯示說明。 別名會儲存在中繼資料檔案中，並且會以 **load metadata** 命令載入。

## <a name="syntax"></a>語法

```
add alias <aliasname> <aliasvalue>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<aliasname>` | 指定別名。 |
| `<aliasvalue>` | 指定別名的值。 |
| `? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要列出所有陰影，包括其別名，請輸入：

```
list shadows all
```

下列摘錄顯示已指派預設別名（ *VSS_SHADOW_x*）的陰影複製：

```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```

若要將名為 *系統 1* 的新別名指派給此陰影複製，請輸入：

```
add alias System1 %VSS_SHADOW_1%
```

或者，您可以使用陰影複製識別碼來指派別名：

```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [載入中繼資料命令](load-metadata.md)
