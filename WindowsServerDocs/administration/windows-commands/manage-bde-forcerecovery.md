---
title: manage-bde forcerecovery
description: Manage-bde forcerecovery 命令的參考文章，此命令會在重新開機時強制受 BitLocker 保護的磁片磁碟機進入修復模式。
ms.topic: reference
ms.assetid: eecae37c-c9a3-46c5-b615-a0ace1f1d778
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b88aa4bb52ea6bcb50f34a6e8d42c6e14e0b1e59
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622609"
---
# <a name="manage-bde-forcerecovery"></a>manage-bde forcerecovery

重新開機時，強制受 BitLocker 保護的磁片磁碟機進入修復模式。 此命令會從磁片磁碟機中刪除所有信賴平臺模組 (TPM) 相關的金鑰保護裝置。 當電腦重新開機時，只有修復密碼或修復金鑰可以用來解除鎖定磁片磁碟機。

## <a name="syntax"></a>語法

```
manage-bde –forcerecovery <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
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

若要讓 BitLocker 在磁片磁碟機 C 的復原模式下啟動，請輸入：

```
manage-bde –forcerecovery C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
