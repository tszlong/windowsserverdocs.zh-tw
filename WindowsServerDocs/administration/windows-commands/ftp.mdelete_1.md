---
title: ftp mdelete
description: Ftp mdelete 命令的參考文章，它會刪除遠端電腦上的檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 720ae8b5ebb0ef380f6547a85913cd84c6d98c7e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931177"
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

若要刪除*a.exe*和*b.exe*的遠端檔案，請輸入：

```
mdelete a.exe b.exe
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
