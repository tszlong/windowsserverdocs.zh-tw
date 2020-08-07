---
title: ftp mls
description: Ftp mls 命令的參考文章，它會在遠端目錄中顯示檔案和子目錄的縮寫清單。
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c2e0dc4ff4b516cc436e5b8a0a9c5ca2e4d77b5d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889234"
---
# <a name="ftp-mls"></a>ftp mls

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
