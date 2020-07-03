---
title: ftp mget
description: Ftp mget 命令的參考文章，它會使用目前的檔案傳輸類型，將遠端檔案複製到本機電腦。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7239f31fa0a9b40d9bba4db0c3e1e29df5c03108
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925893"
---
# <a name="ftp-mget"></a>ftp mget

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將遠端檔案複製到本機電腦。

## <a name="syntax"></a>語法

```
mget <remotefile>[ ]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<remotefile>` | 指定要複製到本機電腦的遠端檔案。 |

### <a name="examples"></a>範例

若要使用目前的檔案傳輸類型，將遠端檔案複製*a.exe*並*b.exe*到本機電腦，請輸入：

```
mget a.exe b.exe
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp ascii 命令](ftp-ascii.md)

- [ftp 二進位命令](ftp-binary.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
