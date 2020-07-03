---
title: cacls
description: Cacls 命令的參考文章。 此命令已被取代，在未來的 Windows 版本中不保證會受到支援。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7719728f2c1cb7ce629e199a51ee211ea5781401
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924847"
---
# <a name="cacls"></a>cacls

>[!IMPORTANT]
> 此命令已被取代。 請改用[icacls](icacls.md) 。

顯示或修改指定檔案上的任意存取控制清單（DACL）。

## <a name="syntax"></a>語法

```
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<filename>` | 必要。 顯示指定檔案的 Acl。 |
| /t | 變更目前目錄和所有子目錄中指定檔案的 Acl。 |
| /m | 變更掛接至目錄之磁片區的 Acl。 |
| /l | 適用于符號連結本身，而不是目標。 |
| /s： sddl | 以 SDDL 字串中指定的 Acl 取代 Acl。 這個參數無效，無法搭配 **/e**、 **/g**、 **/r**、 **/p**或 **/d**參數使用。 |
| /e | 編輯 ACL，而不是取代它。 |
| /C | 拒絕存取錯誤之後繼續。 |
| `/g user:<perm>` | 授與指定的使用者存取權限，包括下列許可權的有效值：<ul><li>**n** -無</li><li>**r** -讀取</li><li>**w** -寫入</li><li>**c** -變更（寫入）</li><li>**f** -完全控制</li></ul> |
| /r 使用者 [...] | 撤銷指定使用者的存取權限。 只有在搭配 **/e**參數使用時才有效。 |
| `[/p user:<perm> [...]` | 取代指定的使用者存取權限，包括下列許可權的有效值：<ul><li>**n** -無</li><li>**r** -讀取</li><li>**w** -寫入</li><li>**c** -變更（寫入）</li><li>**f** -完全控制</li></ul> |
| [/d user [...] | 拒絕指定的使用者存取權。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="sample-output"></a>範例輸出

| 輸出 | 存取控制專案（ACE）適用于 |
-------- | ------------------------------------- |
| OI | 物件繼承。 此資料夾和檔案。 |
| CI | 容器繼承。 這個資料夾和子資料夾。 |
| IO | 僅繼承。 ACE 不會套用至目前的檔案/目錄。 |
| 沒有輸出訊息 | 僅此資料夾。 |
| OICI | 這個資料夾、子資料夾和檔案。 |
| OICI輸出 | 僅限子資料夾和檔案。 |
| CI輸出 | 僅限子資料夾。 |
| OI輸出 | 僅限檔案。 |

#### <a name="remarks"></a>備註

- 您可以使用萬用字元（**？** 和 **&#42;**）來指定多個檔案。

- 您可以指定一個以上的使用者。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [icacls](icacls.md)
