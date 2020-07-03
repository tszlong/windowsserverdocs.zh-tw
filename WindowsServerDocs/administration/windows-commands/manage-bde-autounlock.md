---
title: manage-bde 再次
description: 再次命令的參考文章，它會管理受 BitLocker 保護之資料磁片磁碟機的自動解除鎖定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a81d3e864a33efd5a6a1c81a5a193338d2c25bfa
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931636"
---
# <a name="manage-bde-autounlock"></a>manage-bde 再次

管理受 BitLocker 保護之資料磁片磁碟機的自動解除鎖定。

## <a name="syntax"></a>語法

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| -enable | 啟用資料磁片磁碟機的自動解除鎖定。 |
| -disable | 停用資料磁片磁碟機的自動解除鎖定。 |
| -clearallkeys | 移除作業系統磁片磁碟機上的所有已儲存的外部金鑰。 |
| `<drive>` | 表示後面接著冒號的磁碟機號。 |
| -computername | 指定 manage-bde.exe 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整的說明。 |

## <a name="examples"></a>範例

若要啟用資料磁片磁碟機 E 的自動解除鎖定，請輸入：

```
manage-bde –autounlock -enable E:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)