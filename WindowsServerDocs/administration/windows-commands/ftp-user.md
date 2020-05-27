---
title: ftp 使用者
description: Ftp 使用者命令的參考主題，其指定遠端電腦的使用者。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0773084ee718db37d6c79009d66d754283f94c8
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820258"
---
# <a name="ftp-user"></a>ftp 使用者

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

指定遠端電腦的使用者。

## <a name="syntax"></a>語法

```
user <username> [<password>] [<account>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<username>` | 指定用來登入遠端電腦的使用者名稱。 |
| `[<password>]` | 指定使用者*名稱*的密碼。 如果未指定密碼，但這是必要的， **ftp**命令會提示您輸入密碼。 |
| `[<account>]` | 指定用來登入遠端電腦的帳戶。 如果未指定*帳戶*，但這是必要的， **ftp**命令會提示您輸入該帳戶。 |

### <a name="examples"></a>範例

若要指定具有密碼*Password1*的*User1* ，請輸入：

```
user User1 Password1
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
