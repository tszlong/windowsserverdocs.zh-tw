---
title: manage-bde 鎖定
description: Manage-bde lock 命令的參考文章，它會鎖定受 BitLocker 保護的磁片磁碟機以防止其存取，除非提供解除鎖定金鑰。
ms.topic: article
ms.assetid: b8858e61-3a7e-4d03-8c98-5c09853f35e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a1c7fd743832caaacec46ff2fdc7008983b8472
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886852"
---
# <a name="manage-bde-lock"></a>manage-bde 鎖定

鎖定受 BitLocker 保護的磁片磁碟機以防止其存取，除非提供解除鎖定金鑰。

## <a name="syntax"></a>語法

```
manage-bde -lock [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 表示後面接著冒號的磁碟機號。 |
| -computername | 指定 manage-bde.exe 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整的說明。 |

### <a name="examples"></a>範例

若要鎖定資料磁片磁碟機 D，請輸入：

```
manage-bde –lock D:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
