---
title: ftp mdir
description: Ftp mdir 命令的參考文章，此命令會顯示遠端目錄中檔案和子目錄的目錄清單。
ms.topic: reference
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f5666157032df499309118955a8f39b668ee980
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035566"
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
