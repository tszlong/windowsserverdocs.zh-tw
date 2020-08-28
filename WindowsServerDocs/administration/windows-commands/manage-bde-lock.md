---
title: manage-bde 鎖定
description: Manage-bde 鎖定命令的參考文章，此命令會鎖定受 BitLocker 保護的磁片磁碟機，以防止其存取，除非提供解除鎖定金鑰。
ms.topic: reference
ms.assetid: b8858e61-3a7e-4d03-8c98-5c09853f35e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db92258ed4aa96402c59f5073784bf01cc82b911
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037666"
---
# <a name="manage-bde-lock"></a>manage-bde 鎖定

鎖定受 BitLocker 保護的磁片磁碟機以防止存取，除非提供解除鎖定金鑰。

## <a name="syntax"></a>語法

```
manage-bde -lock [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

### <a name="examples"></a>範例

若要鎖定資料磁片磁碟機 D，請輸入：

```
manage-bde –lock D:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
