---
title: manage-bde setidentifier
description: Setidentifier 命令的參考文章，它會將磁片磁碟機上的 [磁片磁碟機識別碼] 欄位設定為 [為您的組織提供唯一識別碼] 群組原則設定中指定的值。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d20120bf4c6ec76fa6ba040141afadea2a748d5
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922256"
---
# <a name="manage-bde-setidentifier"></a>manage-bde setidentifier

將磁片磁碟機上的磁片磁碟機識別碼欄位設定為 [為**您的組織提供唯一識別碼**] 群組原則設定中指定的值。

## <a name="syntax"></a>語法

```
manage-bde –setidentifier <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<drive>` | 表示後面接著冒號的磁碟機號。 |
| -computername | 指定 manage-bde.exe 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整的說明。 |

### <a name="examples"></a>範例

若要設定 C 的 [BitLocker 磁片磁碟機識別碼] 欄位，請輸入：

```
manage-bde –setidentifier C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)

- [BitLocker 修復指南](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-recovery-guide-plan)
