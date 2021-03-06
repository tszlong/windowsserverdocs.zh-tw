---
title: ksetup setcomputerpassword
description: '>ksetup setcomputerpassword 命令的參考文章，它會設定本機電腦的密碼。'
ms.topic: reference
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9bd9d9fafae57c1c7804f7a214d9729cdb232b2d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627431"
---
# <a name="ksetup-setcomputerpassword"></a>ksetup setcomputerpassword

設定本機電腦的密碼。 此命令只會影響電腦帳戶，需要重新開機密碼變更才會生效。

> [!IMPORTANT]
> 電腦帳戶密碼不會顯示在登錄中，也不會顯示為 **>ksetup** 命令的輸出。

## <a name="syntax"></a>語法

```
ksetup /setcomputerpassword <password>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<password>` | 指定提供的密碼，以在本機電腦上設定電腦帳戶。 密碼只能使用具有系統管理許可權的帳戶來設定，而且密碼必須介於1到156個英數位元或特殊字元。 |

### <a name="examples"></a>範例

若要將本機電腦上的電腦帳戶密碼從 *IPops897* 變更為 *IPop $ 897！*，請輸入：

```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)
