---
title: ksetup setcomputerpassword
description: Ksetup setcomputerpassword 命令的參考主題，其會設定本機電腦的密碼。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ec410cd85c13cb3a925c3fc65b8c9f86fba606a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817428"
---
# <a name="ksetup-setcomputerpassword"></a>ksetup setcomputerpassword

設定本機電腦的密碼。 此命令只會影響電腦帳戶，需要重新開機，密碼變更才會生效。

> [!IMPORTANT]
> 電腦帳戶密碼不會顯示在登錄中，或做為**ksetup**命令的輸出。

## <a name="syntax"></a>語法

```
ksetup /setcomputerpassword <password>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<password>` | 指定在本機電腦上設定電腦帳戶所提供的密碼。 只能使用具有系統管理許可權的帳戶來設定密碼，而且密碼必須是1到156英數位元或特殊字元。 |

### <a name="examples"></a>範例

若要將本機電腦上的電腦帳戶密碼從*IPops897*變更為*IPop $ 897！*，請輸入：

```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)
