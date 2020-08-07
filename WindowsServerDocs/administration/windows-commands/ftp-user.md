---
title: ftp user
description: Ftp 使用者命令的參考文章，其指定遠端電腦的使用者。
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd015b7f84a6f5a4f3ee10a3cbe351a5bfa4a563
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888819"
---
# <a name="ftp-user"></a>ftp user

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

指定遠端電腦的使用者。

## <a name="syntax"></a>語法

```
user <username> [<password>] [<account>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<username>` | 指定用來登入遠端電腦的使用者名稱。 |
| `[<password>]` | 指定使用者*名稱*的密碼。 如果未指定密碼，但這是必要的， **ftp**命令會提示您輸入密碼。 |
| `[<account>]` | 指定用來登入遠端電腦的帳戶。 如果未指定*帳戶*，但這是必要的， **ftp**命令會提示您輸入該帳戶。 |

### <a name="examples"></a>範例

若要指定具有密碼*Password1*的*User1* ，請輸入：

```
user User1 Password1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
