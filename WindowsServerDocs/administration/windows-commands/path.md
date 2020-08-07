---
title: path
description: 在 PATH 環境變數中設定命令路徑的參考文章，指定用來搜尋可執行檔 ( .exe) 檔案的目錄集。
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c81dfef09b4c9a411db9469ec851d4f92180f1d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885100"
---
# <a name="path"></a>path

在 PATH 環境變數中設定命令路徑，指定用來搜尋可執行檔 ( .exe) 檔案的目錄集。 如果使用時不含參數，此命令會顯示目前的命令路徑。

## <a name="syntax"></a>語法

```
path [[<drive>:]<path>[;...][;%PATH%]]
path ;
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[<drive>:]<path>` | 指定要在命令路徑中設定的磁片磁碟機和目錄。 在命令路徑中指定的目錄之前，一律會搜尋目前的目錄。 |
| ; | 在命令路徑中分隔目錄。 如果使用時沒有其他參數，**則**會從 PATH 環境變數清除現有的命令路徑，並將 Cmd.exe 導向僅在目前目錄中搜尋。 |
| `%PATH%` | 將命令路徑附加到 PATH 環境變數中所列的現有目錄集。 如果您包含這個參數，Cmd.exe 會將它取代為 PATH 環境變數中找到的命令路徑值，而不需要在命令提示字元中手動輸入這些值。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註


- Windows 作業系統會依下列優先順序使用預設副檔名來搜尋： .exe、.com、.bat 和 .cmd。 這表示如果您要尋找名為的批次檔，acct.bat，但是在相同的目錄中有一個名為 acct.exe 的應用程式，您必須在命令提示字元中包含 .bat 副檔名。

- 如果命令路徑中有兩個或多個檔案具有相同的檔案名和副檔名，此命令會先在目前目錄中搜尋指定的檔案名。 然後，它會依照 PATH 環境變數中列出的順序，搜尋命令路徑中的目錄。

- 如果您將**path**命令放在 Autoexec 檔案中，Windows 作業系統會在您每次登入電腦時，自動附加指定的 MS-DOS 子系統搜尋路徑。 Cmd.exe 不會使用 Autoexec nt 檔案。 從快捷方式啟動時，Cmd.exe 會繼承我的電腦/Properties/Advanced/環境中設定的環境變數。

## <a name="examples"></a>範例

若要搜尋路徑*c:\user\taxes*、 *b:\user\invest*和*b:\bin*中的外部命令，請輸入：

```
path c:\user\taxes;b:\user\invest;b:\bin
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
