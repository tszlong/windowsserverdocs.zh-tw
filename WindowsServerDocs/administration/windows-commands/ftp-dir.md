---
title: ftp dir
description: Ftp dir 命令的參考文章，此命令會顯示遠端電腦上的目錄檔案和子目錄清單。
ms.topic: reference
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8641fdca55976eb5998cdfbba58eddd3e6d8f286
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621779"
---
# <a name="ftp-dir"></a>ftp dir

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端電腦上的目錄檔案和子目錄清單。

## <a name="syntax"></a>語法

```
dir [<remotedirectory>] [<localfile>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| `[<remotedirectory>]` | 指定您想要查看清單的目錄。 如果未指定目錄，則會使用遠端電腦上的目前工作目錄。 |
| `[<localfile>]` | 指定要在其中儲存目錄清單的本機檔案。 如果未指定本機檔案，結果會顯示在畫面上。 |

### <a name="examples"></a>範例

若要在遠端電腦上顯示 *dir1* 的目錄清單，請輸入：

```
dir dir1
```

若要在本機檔案 *dirlist.txt*中儲存遠端電腦上當前目錄的清單，請輸入：

```
dir . dirlist.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
