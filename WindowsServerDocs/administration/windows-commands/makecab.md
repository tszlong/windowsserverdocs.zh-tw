---
title: makecab
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a0bf4afda09f0e0e8416777a2cfd1404d4bf59a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724225"
---
# <a name="makecab"></a>makecab

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將現有的檔案封裝成封包檔（.cab）。
## <a name="syntax"></a>語法
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
#### <a name="parameters"></a>參數

|      參數       |                                                                        描述                                                                        |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|       <source>       |                                                                     要壓縮的檔案。                                                                     |
|    <destination>     | 要提供壓縮檔案的檔案名。 如果省略，則會以底線（_）取代原始程式檔名稱的最後一個字元，並使用做為目的地。 |
| /f <directives_file> |                                                   具有**makecab.exe**指示詞的檔案（可能會重複）。                                                   |
|    /d var =<value>    |                                                          定義具有指定值的變數。                                                           |
|       /l<dir>       |                                               要放置目的地的位置（預設值是目前的目錄）。                                               |
|       /v [<n>]        |                                                    設定調試層級的詳細等級（0 = 無,..., 3 = 完整）。                                                     |
|          /?          |                                                           在命令提示字元顯示說明。                                                            |

## <a name="remarks"></a>備註
-   如需 directive_file 的詳細資訊，請參閱 MSDN 上的[Microsoft 封包格式](https://go.microsoft.com/fwlink/?LinkId=226852)。

## <a name="additional-references"></a>其他參考
-   - [命令列語法關鍵](command-line-syntax-key.md)

