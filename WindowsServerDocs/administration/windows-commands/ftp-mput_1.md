---
title: ftp mput
description: Ftp mput 命令的參考主題，會使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5006c1ba19f0e017dea377b47bd0d89a68266382
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820408"
---
# <a name="ftp-mput"></a>ftp mput

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。

## <a name="syntax"></a>語法

```
mput <localfile>[ ]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<localfile>` | 指定要複製到遠端電腦的本機檔案。 |

### <a name="examples"></a>範例

若要使用目前的檔案傳輸類型，將*Program1*和*program2.c*複製到遠端電腦，請輸入：

```
mput Program1.exe Program2.exe
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp ascii 命令](ftp-ascii.md)

- [ftp 二進位命令](ftp-binary.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
