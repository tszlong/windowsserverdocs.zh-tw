---
title: makecab
description: Makecab.exe 命令的參考文章，此命令會將現有檔案封裝 ( .cab) 檔案中。
ms.topic: reference
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3ba4adf0d1cc94fc3fc1c5d4db15f641a5c30b90
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640175"
---
# <a name="makecab"></a>makecab

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將現有檔案封裝成封包 ( .cab) 檔。


> [!NOTE]
> 此命令與 [diantz 命令](diantz.md)相同。

## <a name="syntax"></a>語法

```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<source>` | 壓縮檔案。 |
| `<destination>` | 要提供壓縮檔案的檔案名。 如果省略，則會將原始程式檔名稱的最後一個字元取代為底線 (_) ，並做為目的地使用。 |
| /f `<directives_file>` | 具有 **makecab.exe** 指示詞 (的檔案可能會) 重複。 |
| /d var =`<value>` | 使用指定的值定義變數。 |
| /l `<dir>` | 放置目的地 (預設為目前目錄) 的位置。 |
| /v [ `<n>` ] | 設定調試詳細資訊層級 (0 = 無,..., 3 = 完整) 。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [diantz 命令](diantz.md)

- [Microsoft 封包格式](/previous-versions/bb417343(v=msdn.10))
