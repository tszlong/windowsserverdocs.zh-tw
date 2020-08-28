---
title: ftp mput
description: Ftp mput 命令的參考文章，此命令會使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。
ms.topic: reference
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9b90cc6454e64e74684a44f3d69149886c6778a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032846"
---
# <a name="ftp-mput"></a>ftp mput

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。

## <a name="syntax"></a>語法

```
mput <localfile>[ ]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<localfile>` | 指定要複製到遠端電腦的本機檔案。 |

### <a name="examples"></a>範例

若要使用目前的檔案傳輸類型，將 *Program1.exe* 和 *Program2.exe* 複製到遠端電腦，請輸入：

```
mput Program1.exe Program2.exe
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp ascii 命令](ftp-ascii.md)

- [ftp 二進位命令](ftp-binary.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
