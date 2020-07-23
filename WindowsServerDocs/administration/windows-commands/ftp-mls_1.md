---
title: ftp mls
description: Ftp mls 命令的參考文章，它會在遠端目錄中顯示檔案和子目錄的縮寫清單。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f3d5c6b595cf6f8261a4af742cef60d0c9e3abd
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957650"
---
# <a name="ftp-mls"></a>ftp mls

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端目錄中的檔案和子目錄的縮寫清單。

## <a name="syntax"></a>語法

```
mls <remotefile>[ ] <localfile>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<remotefile>` | 指定您要查看其清單的檔案。 指定*remotefiles*時，請使用連字號來代表遠端電腦上的目前工作目錄。 |
| `<localfile>` | 指定用來儲存清單的本機檔案。 指定*localfile*時，請使用連字號來顯示幕幕上的清單。 |

### <a name="examples"></a>範例

若要顯示*dir1*和*dir2*的檔案和子目錄的縮寫清單，請輸入：

```
mls dir1 dir2 -
```

若要在本機檔案*dirlist.txt*中儲存*dir1*和*dir2*的檔案和子目錄的縮寫清單，請輸入：

```
mls dir1 dir2 dirlist.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
