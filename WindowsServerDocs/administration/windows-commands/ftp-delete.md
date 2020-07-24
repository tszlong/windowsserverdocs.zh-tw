---
title: ftp delete
description: Ftp delete 命令的參考文章，它會刪除遠端電腦上的檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 067c45f3-e4e8-4450-b8b6-836994f6adfe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa24e8a052d2eb05d180bce2e843e34532a453f8
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957900"
---
# <a name="ftp-delete"></a>ftp delete

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

刪除遠端電腦上的檔案。

## <a name="syntax"></a>語法

```
delete <remotefile>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<remotefile>` | 指定要刪除的檔案。 |

### <a name="examples"></a>範例

若要刪除遠端電腦上的*test.txt*檔案，請輸入：

```
delete test.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
