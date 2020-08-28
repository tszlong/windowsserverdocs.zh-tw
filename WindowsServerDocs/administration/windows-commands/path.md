---
title: path
description: 在 PATH 環境變數中設定命令路徑的參考文章，指定用來搜尋可執行檔 ( .exe) 檔的目錄集合。
ms.topic: reference
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe60518a70f4fdc9992f70b3b561b067404a31f1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032505"
---
# <a name="path"></a>path

在 PATH 環境變數中設定命令路徑，並指定用來搜尋可執行檔 ( .exe) 檔的目錄集合。 如果使用時不含參數，此命令會顯示目前的命令路徑。

## <a name="syntax"></a>語法

```
path [[<drive>:]<path>[;...][;%PATH%]]
path ;
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[<drive>:]<path>` | 指定要在命令路徑中設定的磁片磁碟機和目錄。 在命令路徑中指定的目錄之前，一律會搜尋目前的目錄。 |
| ; | 分隔命令路徑中的目錄。 如果使用時沒有其他參數， **則** 會從 PATH 環境變數中清除現有的命令路徑，並將 Cmd.exe 導向僅在目前的目錄中搜尋。 |
| `%PATH%` | 將命令路徑附加至 PATH 環境變數中所列的現有目錄集。 如果您包含此參數，Cmd.exe 會使用 PATH 環境變數中找到的命令路徑值來取代它，而不需要在命令提示字元中手動輸入這些值。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註


- Windows 作業系統會依照下列優先順序來搜尋使用預設的副檔名： .exe、.com、.bat 和 .cmd。 這表示，如果您要尋找名為 acct.bat 的批次檔，但在相同目錄中有一個名為 acct.exe 的應用程式，則必須在命令提示字元中包含 .bat 副檔名。

- 如果命令路徑中有兩個以上的檔案具有相同的檔案名和副檔名，此命令會先在目前的目錄中搜尋指定的檔案名。 然後，它會依照 PATH 環境變數中所列的順序，在命令路徑中搜尋目錄。

- 如果您將 **path** 命令放在 Autoexec nt 檔案中，Windows 作業系統會在您每次登入電腦時自動附加指定的 MS-DOS 子系統搜尋路徑。 Cmd.exe 不會使用 Autoexec nt 檔案。 從快捷方式啟動時，Cmd.exe 繼承我的電腦/Properties/Advanced/環境中設定的環境變數。

## <a name="examples"></a>範例

若要在路徑中搜尋外部命令的 *c:\user\taxes*、 *b:\user\invest*和 *b:\bin* ，請輸入：

```
path c:\user\taxes;b:\user\invest;b:\bin
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
