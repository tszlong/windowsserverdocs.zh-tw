---
title: manage-bde setidentifier
description: Manage-bde setidentifier 命令的參考文章，此命令會將磁片磁碟機上的磁片磁碟機識別碼欄位設定為 [為您的組織提供唯一識別碼] 群組原則設定中指定的值。
ms.topic: reference
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f790efb56ddb3c8785d89930069c5be42e7a5b0c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027486"
---
# <a name="manage-bde-setidentifier"></a>manage-bde setidentifier

將磁片磁碟機上的磁片磁碟機識別碼欄位設定為 [為 **您的組織提供唯一識別碼** ] 群組原則設定中指定的值。

## <a name="syntax"></a>語法

```
manage-bde –setidentifier <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
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

若要設定 C 的 BitLocker 磁片磁碟機識別碼欄位，請輸入：

```
manage-bde –setidentifier C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)

- [BitLocker 修復指南](/windows/security/information-protection/bitlocker/bitlocker-recovery-guide-plan)
