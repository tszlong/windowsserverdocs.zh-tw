---
title: ftp put
description: Ftp put 命令的參考文章，它會使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/30/2020
ms.openlocfilehash: 5a1a262957177efd6e393f22c9fd9b5541650666
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957580"
---
# <a name="ftp-put"></a>ftp put

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。

> [!NOTE]
> 此命令與[ftp send 命令](ftp-send_1.md)相同。

## <a name="syntax"></a>語法

```
put <localfile> [<remotefile>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<localfile>` | 指定要複製的本機檔案。 |
| `[<remotefile>]` | 指定要在遠端電腦上使用的名稱。 如果您未指定*remotefile*，檔案就會提供*localfile*名稱。|

### <a name="examples"></a>範例

若要複製本機檔案*test.txt*並將它命名為*test1.txt*在遠端電腦上，請輸入：

```
put test.txt test1.txt
```

若要將本機檔案*program.exe*複製到遠端電腦，請輸入：

```
put program.exe
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp ascii 命令](ftp-ascii.md)

- [ftp 二進位命令](ftp-binary.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
