---
title: md
description: Md 命令的參考文章，此命令會建立目錄或子目錄。
ms.topic: reference
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3da07c6e9d2e6fd7499f74e8ef10fdf3ba85586
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037846"
---
# <a name="md"></a>md

建立目錄或子目錄。 預設會啟用的命令延伸模組可讓您使用單一 **md** 命令，在指定的路徑中建立中繼目錄。

> [!NOTE]
> 此命令與 [mkdir 命令](mkdir.md)相同。

## <a name="syntax"></a>語法

```
md [<drive>:]<path>
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
md Directory1
```

若要在根目錄內建立目錄樹狀結構 *Taxes\Property\Current* ，並啟用命令延伸模組，請輸入：

```
md \Taxes\Property\Current
```

如先前的範例所示，在根目錄內建立目錄樹狀結構 *Taxes\Property\Current* ，但已停用命令延伸模組，請輸入下列命令順序：

```
md \Taxes
md \Taxes\Property
md \Taxes\Property\Current
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [mkdir 命令](mkdir.md)