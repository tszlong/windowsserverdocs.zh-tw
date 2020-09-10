---
title: ftp mdir
description: Ftp mdir 命令的參考文章，此命令會顯示遠端目錄中檔案和子目錄的目錄清單。
ms.topic: reference
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 40c64d82f07a763dec3dd690780438eb2db35753
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636665"
---
# <a name="ftp-mdir"></a>ftp mdir

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端目錄中檔案和子目錄的目錄清單。

## <a name="syntax"></a>語法

```
mdir <remotefile>[...] <localfile>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<remotefile>` | 指定您想要查看清單的目錄或檔案。 您可以指定多個 *remotefiles*。 輸入連字號 (-) ，以使用遠端電腦上的目前工作目錄。 |
| `<localfile>` | 指定要儲存清單的本機檔案。 此為必要參數。 輸入連字號 (-) ，以顯示幕幕上的清單。 |

### <a name="examples"></a>範例

若要在螢幕上顯示 *dir1* 和 *dir2* 的目錄清單，請輸入：

```
mdir dir1 dir2 -
```

若要將 *dir1* 和 *dir2* 的合併目錄清單儲存在稱為 *dirlist.txt*的本機檔案中，請輸入：

```
mdir dir1 dir2 dirlist.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
