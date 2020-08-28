---
title: CD
description: Cd 命令的參考文章，此命令會顯示或變更目前目錄的名稱。
ms.topic: reference
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef5f6f247702c96b3dcca0bda7596ae43867d9e7
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034196"
---
# <a name="cd"></a>CD

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示目前目錄的名稱，或變更目前的目錄。 如果只搭配磁碟機號使用 (例如 `cd C:`) ， **cd** 會在指定的磁片磁碟機中顯示目前目錄的名稱。 如果使用時不含參數， **cd** 會顯示目前的磁片磁碟機和目錄。

> [!NOTE]
> 此命令與 [chdir 命令](chdir.md)相同。

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
| /d | 變更目前磁片磁碟機以及磁片磁碟機的目前的目錄。 |
| `<drive>:` | 指定要顯示或變更 (（如果與目前磁片磁碟機) 不同）的磁片磁碟機。 |
| `<path>` | 指定您想要顯示或變更之目錄的路徑。 |
| [..] | 指定您要變更為上層資料夾。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

如果啟用了命令延伸模組，下列條件適用于 **cd** 命令：

- 目前的目錄字串會轉換為使用與磁片上的名稱相同的大小寫。 例如， `cd c:\temp` 如果磁片上的情況是，則會將目前的目錄設為 C：\Temp。

- 空格不會被視為分隔符號，因此 `<path>` 可以包含空格，而不會加上引號。 例如：

  ```
  cd username\programs\start menu
  ```

  就如同：

  ```
  cd "username\programs\start menu"
  ```

  如果已停用延伸模組，則必須加上引號。

- 若要停用命令延伸模組，請輸入：

  ```
  cmd /e:off
  ```

## <a name="examples"></a>範例

若要返回根目錄，磁片磁碟機的目錄階層頂端：

```
cd\
```

若要變更不同于您所在磁片磁碟機上的預設目錄：

```
cd [<drive>:[<directory>]]
```

若要確認目錄的變更，請輸入：

```
cd [<drive>:]
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [chdir 命令](chdir.md)
