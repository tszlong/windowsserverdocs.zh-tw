---
title: manage-bde forcerecovery
description: Forcerecovery 命令的參考文章，它會在重新開機時強制受 BitLocker 保護的磁片磁碟機進入修復模式。
ms.topic: article
ms.assetid: eecae37c-c9a3-46c5-b615-a0ace1f1d778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 407ec574c66c057664d517bda35b82da908e0291
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886886"
---
# <a name="manage-bde-forcerecovery"></a>manage-bde forcerecovery

重新開機時，強制將受 BitLocker 保護的磁片磁碟機進入修復模式。 此命令會從磁片磁碟機刪除與 TPM) 相關的金鑰保護裝置 (的所有信賴平臺模組。 當電腦重新開機時，只會使用修復密碼或修復金鑰來解除鎖定磁片磁碟機。

## <a name="syntax"></a>語法

```
manage-bde –forcerecovery <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
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

若要讓 BitLocker 在磁片磁碟機 C 上以修復模式啟動，請輸入：

```
manage-bde –forcerecovery C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
