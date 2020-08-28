---
title: mkdir
description: Mkdir 命令的參考文章，此命令會建立目錄或子目錄。
ms.topic: reference
ms.assetid: 033a57a2-5deb-4c98-aa78-61ce8df2a330
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ded16e2befe952541dfaac754b0d10c7c128f0a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037836"
---
# <a name="mkdir"></a>mkdir

建立目錄或子目錄。 預設會啟用的命令延伸模組可讓您使用單一 **mkdir** 命令，在指定的路徑中建立中繼目錄。

> [!NOTE]
> 此命令與 [md 命令](md.md)相同。

## <a name="syntax"></a>語法

```
mkdir [<drive>:]<path>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>`: | 指定您要在其中建立新目錄的磁片磁碟機。 |
| `<path>` | 指定新目錄的名稱和位置。 任何單一路徑的最大長度都是由檔案系統所決定。 這是必要參數。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要在目前的目錄中建立名為 *Directory1* 的目錄，請輸入：

```
mkdir Directory1
```

若要在根目錄內建立目錄樹狀結構 *Taxes\Property\Current* ，並啟用命令延伸模組，請輸入：

```
mkdir \Taxes\Property\Current
```

如先前的範例所示，在根目錄內建立目錄樹狀結構 *Taxes\Property\Current* ，但已停用命令延伸模組，請輸入下列命令順序：

```
mkdir \Taxes
mkdir \Taxes\Property
mkdir \Taxes\Property\Current
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [md 命令](md.md)