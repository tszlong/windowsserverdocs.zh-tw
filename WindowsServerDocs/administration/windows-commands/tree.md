---
title: tree
description: 樹狀目錄的參考文章，以圖形方式顯示路徑或磁片磁碟機中磁片的目錄結構。
ms.topic: reference
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c9f586dbcf9072feffe71e1c3d792f10afc951a4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156404"
---
# <a name="tree"></a>tree

以圖形方式顯示磁片磁碟機路徑或磁片的目錄結構。 此命令所顯示的結構取決於您在命令提示字元中指定的參數。 如果您未指定磁片磁碟機或路徑，此命令會顯示以目前磁片磁碟機的目前的目錄開頭的樹狀結構。

## <a name="syntax"></a>語法

```
tree [<drive>:][<path>] [/f] [/a]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<drive>:` | 指定包含您要顯示其目錄結構之磁片的磁片磁碟機。 |
| `<path>` | 指定您想要顯示目錄結構的目錄。 |
| /f | 顯示每個目錄中檔案的名稱。 |
| /a | 指定使用文字字元（而不是圖形字元）來顯示連結子目錄的行。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要顯示目前磁片磁碟機中磁片上所有子目錄的名稱，請輸入：

```
tree \
```

若要一次顯示一個畫面、磁片磁碟機 C 上所有目錄中的檔案，請輸入：

```
tree c:\ /f | more
```

若要列印磁片磁碟機 C 上所有目錄的清單，請輸入：

```
tree c:\ /f  prn
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
