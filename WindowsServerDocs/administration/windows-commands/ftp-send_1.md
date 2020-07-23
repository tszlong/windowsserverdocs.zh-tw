---
title: ftp send
description: Ftp send 命令的參考文章，它會使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a734e20e2650a064b6dc293bae96a013b6e1f22
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957400"
---
# <a name="ftp-send"></a>ftp send

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。

> [!NOTE]
> 此命令與[ftp put 命令](ftp-put.md)相同。

## <a name="syntax"></a>語法

```
send <localfile> [<remotefile>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<localfile>` | 指定要複製的本機檔案。 |
| `<remotefile>` | 指定要在遠端電腦上使用的名稱。 如果您未指定*remotefile*，檔案將會取得*localfile*名稱。 |

### <a name="examples"></a>範例

若要複製本機檔案*test.txt*並將它命名為*test1.txt*在遠端電腦上，請輸入：

```
send test.txt test1.txt
```

若要將本機檔案*program.exe*複製到遠端電腦，請輸入：

```
send program.exe
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
