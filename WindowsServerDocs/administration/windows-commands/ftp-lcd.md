---
title: ftp lcd
description: Ftp lcd 命令的參考文章，此命令會變更本機電腦上的工作目錄。
ms.topic: reference
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 91b2990495c91033c1bc885ec9d19142640f55b5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621671"
---
# <a name="ftp-lcd"></a>ftp lcd

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更本機電腦上的工作目錄。 根據預設，工作目錄是用來啟動 **ftp** 命令的目錄。

## <a name="syntax"></a>語法

```
lcd [<directory>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `[<directory>]` | 指定本機電腦上要變更的目錄。 如果未指定 *目錄* ，則會將目前的工作目錄變更為預設目錄。 |

### <a name="examples"></a>範例

若要將本機電腦上的工作目錄變更為 *c:\dir1*，請輸入：

```
lcd c:\dir1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
