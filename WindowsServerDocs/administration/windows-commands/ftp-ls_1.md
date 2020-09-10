---
title: ftp ls
description: Ftp ls 命令的參考文章，此命令會顯示遠端電腦的檔案和子目錄的縮寫清單。
ms.topic: reference
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 845d8fed8dbb637a241d4b1c4491ef9f037eceb8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621611"
---
# <a name="ftp-ls"></a>ftp ls

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端電腦的檔案和子目錄的縮寫清單。

## <a name="syntax"></a>語法

```
ls [<remotedirectory>] [<localfile>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |------------ |
| `[<remotedirectory>]` | 指定您想要查看清單的目錄。 如果未指定目錄，則會使用遠端電腦上的目前工作目錄。 |
| `[<localfile>]` | 指定要在其中儲存清單的本機檔案。 如果未指定本機檔案，結果會顯示在畫面上。 |

### <a name="examples"></a>範例

若要顯示遠端電腦的檔案和子目錄的縮寫清單，請輸入：

```
ls
```

若要取得遠端電腦上 *dir1* 的縮寫目錄清單，並將它儲存在名為 *dirlist.txt*的本機檔案中，請輸入：

```
ls dir1 dirlist.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
