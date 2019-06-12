---
title: path
description: 了解如何設定 PATH 環境變數。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a637abc91dd3342afb3a2723d1b3a835be149122
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436325"
---
# <a name="path"></a>path



設定命令路徑在 PATH 環境變數 （用來搜尋可執行檔的目錄集）。 如果未指定參數，使用**路徑**顯示目前的命令路徑。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

## <a name="parameters"></a>參數

|     參數     |                                                                                                     描述                                                                                                      |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:]<Path> |                                                                            指定的磁碟機和目錄中的命令路徑設定。                                                                             |
|         ;         | 區隔中的命令路徑的目錄。 如果未指定其他參數，使用 **;** 清除現有的命令路徑，從 PATH 環境變數，並指示只在目前的目錄中搜尋的 Cmd.exe。 |
|      %PATH%       |                                                         將命令路徑附加至現有的 PATH 環境變數中所列的目錄集合中。                                                         |
|        /?         |                                                                                         在命令提示字元顯示說明。                                                                                         |

## <a name="remarks"></a>備註

-   當您包含 **%路徑 %** 在語法中，Cmd.exe 取代它在 PATH 環境變數中，找到的命令路徑值不需要手動輸入這些值，在命令提示字元。
-   命令路徑中指定的目錄之前一律搜尋目前的目錄。
-   您可能擁有的目錄中的檔案共用相同的檔案名稱，但有不同的副檔名。 比方說，您可能有一個名為 Accnt.com 啟動會計程式檔案和名為 Accnt.bat 將伺服器連線帳戶處理系統網路的另一個檔案。

    Windows 作業系統會搜尋檔案以下列順序的優先順序，使用預設檔案名稱副檔名：.exe、.com、.bat、 和。 cmd。 若要 Accnt.bat Accnt.com 已存在時執行相同的目錄中，您必須包含.bat 延伸模組，在命令提示字元。
-   如果命令路徑中的兩個或多個檔案有相同的檔案名稱與擴充功能中，**路徑**目前目錄中的第一個搜尋指定的檔案名稱。 然後它會搜尋目錄中的命令路徑，它們會列在 PATH 環境變數的順序。
-   如果您將放**路徑**命令在 Autoexec.nt 檔案中，Windows 作業系統會自動將附加指定的 MS-DOS 子系統搜尋路徑，每次登入您的電腦。 Cmd.exe 不會使用 Autoexec.nt 檔案。 從捷徑啟動時，Cmd.exe 便會繼承設定 我的電腦/屬性/進階/環境中的環境變數。

## <a name="BKMK_examples"></a>範例

若要搜尋的路徑，C:\User\Taxes、 B:\User\Invest 和 B:\Bin 外部命令，請輸入：

`path c:\user\taxes;b:\user\invest;b:\bin`

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)