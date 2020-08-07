---
title: CD
description: Cd 命令的參考文章，它會顯示或變更目前的目錄的名稱。
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87766cd7be95eeb9cbecd29ec88a044224dc81da
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880387"
---
# <a name="cd"></a>CD

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示目前目錄的名稱，或變更目前的目錄。 如果僅搭配磁碟機號使用 (例如， `cd C:`) ， **cd**會顯示指定磁片磁碟機中目前目錄的名稱。 如果在沒有參數的情況下使用， **cd**會顯示目前的磁片磁碟機和目錄。

> [!NOTE]
> 此命令與[chdir 命令](chdir.md)相同。

## <a name="syntax"></a>語法

```
cd [/d] [<drive>:][<path>]
cd [..]
chdir [/d] [<drive>:][<path>]
chdir [..]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /d | 變更目前的磁片磁碟機以及磁片磁碟機的目前目錄。 |
| `<drive>:` | 指定要顯示或變更 (的磁片磁碟機（如果與目前的磁片磁碟機) 不同）。 |
| `<path>` | 指定您想要顯示或變更之目錄的路徑。 |
| [..] | 指定您想要變更為父資料夾。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

如果已啟用命令延伸模組，則下列條件適用于**cd**命令：

- 目前的目錄字串會轉換成使用與磁片上的名稱相同的大小寫。 例如， `cd c:\temp` 如果磁片上發生這種情況，則會將目前的目錄設為 C：\Temp。

- 空格不會被視為分隔符號，因此 `<path>` 可以包含空格，而不需要括住引號。 例如：

  ```
  cd username\programs\start menu
  ```

  就如同：

  ```
  cd "username\programs\start menu"
  ```

  如果停用延伸模組，則需要用到引號。

- 若要停用命令延伸模組，請輸入：

  ```
  cmd /e:off
  ```

## <a name="examples"></a>範例

若要回到根目錄，磁片磁碟機的目錄階層頂端：

```
cd\
```

若要在不同于您所在的磁片磁碟機上變更預設目錄：

```
cd [<drive>:[<directory>]]
```

若要驗證目錄的變更，請輸入：

```
cd [<drive>:]
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [chdir 命令](chdir.md)
