---
title: ftp 傳送
description: Ftp send 命令的參考主題，會使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12ce45a0eb26e1aa4a0d7daace831751e1b67f4a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820298"
---
# <a name="ftp-send"></a>ftp 傳送

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用目前的檔案傳輸類型，將本機檔案複製到遠端電腦。

> [!NOTE]
> 此命令與[ftp put 命令](ftp-put.md)相同。

## <a name="syntax"></a>語法

```
send <localfile> [<remotefile>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<localfile>` | 指定要複製的本機檔案。 |
| `<remotefile>` | 指定要在遠端電腦上使用的名稱。 如果您未指定*remotefile*，檔案將會取得*localfile*名稱。 |

### <a name="examples"></a>範例

若要複製本機檔案*test.txt* ，並在遠端電腦上將它命名為*test1* ，請輸入：

```
send test.txt test1.txt
```

若要將本機檔案*program*複製到遠端電腦，請輸入：

```
send program.exe
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
