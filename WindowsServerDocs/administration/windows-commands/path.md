---
title: 路徑
description: 瞭解如何設定 PATH 環境變數。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52d4a99d21574f9cae3120201dcd4db0cd9a2202
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723369"
---
# <a name="path"></a>路徑



設定 PATH 環境變數中的命令路徑（用來搜尋可執行檔的目錄集合）。 如果在沒有參數的情況下使用， **path**會顯示目前的命令路徑。



## <a name="syntax"></a>語法

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

### <a name="parameters"></a>參數

|     參數     |                                                                                                     描述                                                                                                      |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<磁片磁碟機>：]<Path> |                                                                            指定要在命令路徑中設定的磁片磁碟機和目錄。                                                                             |
|         ;         | 在命令路徑中分隔目錄。 如果使用時沒有其他參數，**則**會從 PATH 環境變數清除現有的命令路徑，並指示 cmd.exe 只在目前目錄中搜尋。 |
|      路徑名       |                                                         將命令路徑附加到 PATH 環境變數中所列的現有目錄集。                                                         |
|        /?         |                                                                                         在命令提示字元顯示說明。                                                                                         |

## <a name="remarks"></a>備註

-   當您在語法中包含 **% PATH%** 時，cmd.exe 會將它取代為 path 環境變數中找到的命令路徑值，而不需要在命令提示字元中手動輸入這些值。
-   在命令路徑中指定的目錄之前，一律會搜尋目前的目錄。
-   您的目錄中可能有共用相同檔案名但具有不同副檔名的檔案。 例如，您可能會有一個名為 Accnt.com 的檔案，它會啟動一個會計程式，另一個名為 Accnt 的檔案，將您的伺服器連接到帳戶處理系統網路。

    Windows 作業系統會依照下列優先順序，使用預設的副檔名來搜尋檔案： .exe、.com、.bat 和 .cmd。 當 Accnt.com 存在於相同的目錄時，若要執行 Accnt，您必須在命令提示字元中包含 .bat 副檔名。
-   如果命令路徑中有兩個以上的檔案具有相同的檔案名和副檔名，**路徑**會先搜尋目前目錄中指定的檔案名。 然後，它會依照 PATH 環境變數中列出的順序，搜尋命令路徑中的目錄。
-   如果您將**path**命令放在 Autoexec 檔案中，Windows 作業系統會在您每次登入電腦時，自動附加指定的 MS-DOS 子系統搜尋路徑。 Cmd.exe 不會使用 Autoexec 檔案。 從快捷方式啟動時，Cmd.exe 會繼承在我的電腦/Properties/Advanced/環境中設定的環境變數。

## <a name="examples"></a><a name="BKMK_examples"></a>範例

若要搜尋路徑 C:\User\Taxes、B:\User\Invest 和 B:\Bin 中的外部命令，請輸入：

`path c:\user\taxes;b:\user\invest;b:\bin`

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)