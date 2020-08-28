---
title: ftp mls
description: Ftp mls 命令的參考文章，此命令會顯示遠端目錄中的檔案和子目錄的縮寫清單。
ms.topic: reference
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b4eaf9a4b31fb233d281514bcc50ddc754d398a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038026"
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
| `<remotefile>` | 指定您想要查看清單的檔案。 指定 *remotefiles*時，請使用連字號來代表遠端電腦上的目前工作目錄。 |
| `<localfile>` | 指定要在其中儲存清單的本機檔案。 指定 *localfile*時，請使用連字號來顯示畫面上的清單。 |

### <a name="examples"></a>範例

若要顯示 *dir1* 和 *dir2*的檔案和子目錄的縮寫清單，請輸入：

```
mls dir1 dir2 -
```

若要在本機檔案*dirlist.txt*中儲存*dir1*和*dir2*的檔案和子目錄的縮寫清單，請輸入：

```
mls dir1 dir2 dirlist.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
