---
title: manage-bde 再次
description: Manage-bde 再次命令的參考文章，可管理受 BitLocker 保護之資料磁片磁碟機的自動解除鎖定。
ms.topic: reference
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e6035b9d4a851631770b01ba525759d1d61992f5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633755"
---
# <a name="manage-bde-autounlock"></a>manage-bde 再次

管理受 BitLocker 保護之資料磁片磁碟機的自動解除鎖定。

## <a name="syntax"></a>語法

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -enable | 啟用資料磁片磁碟機的自動解除鎖定。 |
| -disable | 停用資料磁片磁碟機的自動解除鎖定。 |
| -clearallkeys | 移除作業系統磁片磁碟機上所有儲存的外部金鑰。 |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

## <a name="examples"></a>範例

若要啟用資料磁片磁碟機 E 的自動解除鎖定，請輸入：

```
manage-bde –autounlock -enable E:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)