---
title: diantz
description: Diantz 命令的參考文章，會將現有的檔案封裝成封包檔（.cab）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 218ed5d7-1203-4d68-ad9b-65cdd022d54f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61a10c2fb67225de1060d64db6fda4e4ff703a7b
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930604"
---
# <a name="diantz"></a>diantz

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將現有的檔案封裝成封包檔（.cab）。 此命令會執行與更新的[makecab.exe 命令](makecab.md)相同的動作。

## <a name="syntax"></a>語法

```
diantz [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
diantz [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<source>` | 要壓縮的檔案。 |
| `<destination>` | 要提供壓縮檔案的檔案名。 如果省略，則會以底線（_）取代原始程式檔名稱的最後一個字元，並使用做為目的地。 |
| /f `<directives_file>` | 具有**diantz**指示詞的檔案（可能會重複）。 |
| /d var =`<value>` | 定義具有指定值的變數。 |
| /l`<dir>` | 要放置目的地的位置（預設值是目前的目錄）。 |
| /v [ `<n>` ] | 設定調試層級的詳細等級（0 = 無,..., 3 = 完整）。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Microsoft 封包格式](https://docs.microsoft.com/previous-versions/bb417343(v=msdn.10))
