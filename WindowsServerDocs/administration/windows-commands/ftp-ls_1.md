---
title: ftp ls
description: Ftp ls 命令的參考主題，它會從遠端電腦顯示檔案和子目錄的縮寫清單。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ae913f001c3ddffce9ff81c9c5c5fd32f436da5
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820168"
---
# <a name="ftp-ls"></a>ftp ls

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從遠端電腦顯示檔案和子目錄的縮寫清單。

## <a name="syntax"></a>語法

```
ls [<remotedirectory>] [<localfile>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- |------------ |
| `[<remotedirectory>]` | 指定您要查看其清單的目錄。 如果未指定任何目錄，則會使用遠端電腦上的目前工作目錄。 |
| `[<localfile>]` | 指定用來儲存清單的本機檔案。 如果未指定本機檔案，結果會顯示在畫面上。 |

### <a name="examples"></a>範例

若要從遠端電腦顯示檔案和子目錄的縮寫清單，請輸入：

```
ls
```

若要取得遠端電腦上*dir1*的縮寫目錄清單，並將它儲存在名為*dirlist*的本機檔案中，請輸入：

```
ls dir1 dirlist.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
