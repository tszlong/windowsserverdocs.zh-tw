---
title: diantz
description: Diantz 命令的參考文章，此命令會將現有檔案封裝 ( .cab) 檔案中。
ms.topic: reference
ms.assetid: 218ed5d7-1203-4d68-ad9b-65cdd022d54f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 033bfb59bbd33fee44bb307d1a905dc6c8d658c7
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635009"
---
# <a name="diantz"></a>diantz

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將現有檔案封裝成封包 ( .cab) 檔。 此命令會執行與更新的 [makecab.exe 命令](makecab.md)相同的動作。

## <a name="syntax"></a>語法

```
diantz [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
diantz [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<source>` | 壓縮檔案。 |
| `<destination>` | 要提供壓縮檔案的檔案名。 如果省略，則會將原始程式檔名稱的最後一個字元取代為底線 (_) ，並做為目的地使用。 |
| /f `<directives_file>` | 具有 **diantz** 指示詞 (的檔案可能會) 重複。 |
| /d var =`<value>` | 使用指定的值定義變數。 |
| /l `<dir>` | 放置目的地 (預設為目前目錄) 的位置。 |
| /v [ `<n>` ] | 設定調試詳細資訊層級 (0 = 無,..., 3 = 完整) 。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Microsoft 封包格式](/previous-versions/bb417343(v=msdn.10))
