---
title: ftp mdelete
description: Ftp mdelete 命令的參考主題，它會刪除遠端電腦上的檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8ee1882878ce06a16bd6ff6f0dcaa512d6d8b56a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820228"
---
# <a name="ftp-mdelete"></a>ftp mdelete

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

刪除遠端電腦上的檔案。

## <a name="syntax"></a>語法
```
mdelete <remotefile>[...]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<remotefile>` | 指定要刪除的遠端檔案。 |

### <a name="examples"></a>範例

若要刪除遠端檔案 *.exe*和*b.*，請輸入：

```
mdelete a.exe b.exe
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
