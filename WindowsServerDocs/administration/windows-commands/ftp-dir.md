---
title: ftp dir
description: Ftp dir 命令的參考文章，它會顯示遠端電腦上的目錄檔案和子目錄清單。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c3ec103797a1683c6f2810da375b00b56b87414
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957870"
---
# <a name="ftp-dir"></a>ftp dir

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端電腦上的目錄檔案和子目錄清單。

## <a name="syntax"></a>語法

```
dir [<remotedirectory>] [<localfile>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| `[<remotedirectory>]` | 指定您要查看其清單的目錄。 如果未指定任何目錄，則會使用遠端電腦上的目前工作目錄。 |
| `[<localfile>]` | 指定用來儲存目錄清單的本機檔案。 如果未指定本機檔案，結果會顯示在畫面上。 |

### <a name="examples"></a>範例

若要在遠端電腦上顯示*dir1*的目錄清單，請輸入：

```
dir dir1
```

若要將遠端電腦上目前目錄的清單儲存在本機檔案*dirlist.txt*中，請輸入：

```
dir . dirlist.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
