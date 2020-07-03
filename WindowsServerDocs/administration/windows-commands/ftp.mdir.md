---
title: ftp mdir
description: Ftp mdir 命令的參考文章，它會在遠端目錄中顯示檔案和子目錄的目錄清單。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a4d1b00941d350776fd953607a5cc5da433993c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926154"
---
# <a name="ftp-mdir"></a>ftp mdir

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端目錄中檔案和子目錄的目錄清單。

## <a name="syntax"></a>語法

```
mdir <remotefile>[...] <localfile>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<remotefile>` | 指定您要查看其清單的目錄或檔案。 您可以指定多個*remotefiles*。 輸入連字號（-），以使用遠端電腦上目前的工作目錄。 |
| `<localfile>` | 指定用來儲存清單的本機檔案。 此為必要參數。 輸入連字號（-）以在螢幕上顯示清單。 |

### <a name="examples"></a>範例

若要在螢幕上顯示*dir1*和*dir2*的目錄清單，請輸入：

```
mdir dir1 dir2 -
```

若要將*dir1*和*dir2*的合併目錄清單儲存在名為*dirlist.txt*的本機檔案中，請輸入：

```
mdir dir1 dir2 dirlist.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
