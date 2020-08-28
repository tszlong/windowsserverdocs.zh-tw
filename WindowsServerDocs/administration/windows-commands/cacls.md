---
title: cacls
description: Cacls 命令的參考文章。 此命令已被取代，而且在未來的 Windows 版本中不保證會受到支援。
ms.topic: reference
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8261e9711cee85ad1d59ff71f9cd8ac55e63fab8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034276"
---
# <a name="cacls"></a>cacls

>[!IMPORTANT]
> 此命令已被取代。 請改用 [icacls](icacls.md) 。

顯示或修改指定檔案 (DACL) 的任意存取控制清單。

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
| /s： sddl | 以 SDDL 字串中指定的 Acl 取代 Acl。 此參數與 **/e**、 **/g**、 **/r**、 **/p**或 **/d** 參數搭配使用無效。 |
| /e | 編輯 ACL，而不是加以取代。 |
| /C | 拒絕存取錯誤後繼續。 |
| `/g user:<perm>` | 授與指定的使用者存取權限，包括下列許可權有效值：<ul><li>**n** -無</li><li>**r** -讀取</li><li>**w** -寫入</li><li>**c** -變更 (寫入) </li><li>**f** -完全控制</li></ul> |
| /r 使用者 [...] | 撤銷指定的使用者存取權限。 只有在搭配 **/e** 參數使用時才有效。 |
| `[/p user:<perm> [...]` | 取代指定的使用者存取權限，包括下列許可權有效值：<ul><li>**n** -無</li><li>**r** -讀取</li><li>**w** -寫入</li><li>**c** -變更 (寫入) </li><li>**f** -完全控制</li></ul> |
| [/d 使用者 [...] | 拒絕指定的使用者存取權。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="sample-output"></a>範例輸出

| 輸出 |  (ACE 的存取控制專案) 適用于 |
-------- | ------------------------------------- |
| OI | 物件繼承。 這個資料夾和檔案。 |
| CI | 容器繼承。 這個資料夾與子資料夾。 |
| IO | 僅繼承。 ACE 並不適用于目前的檔案/目錄。 |
| 沒有輸出訊息 | 只有這個資料夾。 |
|  (OI) # B2 CI)  | 這個資料夾、子資料夾和檔案。 |
|  (OI) # B2 CI) # B4 IO)  | 僅限子資料夾和檔案。 |
|  (CI) # B2 IO)  | 僅限子資料夾。 |
|  (OI) # B2 IO)  | 僅限檔案。 |

#### <a name="remarks"></a>備註

- 您可以使用萬用字元 (**？** 和 **&#42;**) 來指定多個檔案。

- 您可以指定一個以上的使用者。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [icacls](icacls.md)
