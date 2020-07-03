---
title: mkdir
description: Mkdir 命令的參考文章，它會建立目錄或子目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 033a57a2-5deb-4c98-aa78-61ce8df2a330
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7c1569e82143443de861216e40b904de4481a03
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931281"
---
# <a name="mkdir"></a>mkdir

建立目錄或子目錄。 預設會啟用的命令延伸模組，可讓您使用單一**mkdir**命令，在指定的路徑中建立中繼目錄。

> [!NOTE]
> 此命令與[md 命令](md.md)相同。

## <a name="syntax"></a>語法

```
mkdir [<drive>:]<path>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<drive>`: | 指定您要在其上建立新目錄的磁片磁碟機。 |
| `<path>` | 指定新目錄的名稱和位置。 任何單一路徑的最大長度都是由檔案系統所決定。 這是必要參數。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要在目前目錄中建立名為*Directory1*的目錄，請輸入：

```
mkdir Directory1
```

若要建立根目錄內的目錄樹狀結構*Taxes\Property\Current* ，並啟用命令延伸模組，請輸入：

```
mkdir \Taxes\Property\Current
```

如先前範例所示，若要在根目錄中建立目錄樹狀結構*Taxes\Property\Current* ，但已停用命令延伸模組，請輸入下列順序的命令：

```
mkdir \Taxes
mkdir \Taxes\Property
mkdir \Taxes\Property\Current
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [md 命令](md.md)