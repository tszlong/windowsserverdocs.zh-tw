---
title: mountvol
description: Mountvol 命令的參考文章，它會建立、刪除或列出磁片區掛接點。
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1289874f76bd9c3d5818a582a8fcefbfc8c4c130
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886367"
---
# <a name="mountvol"></a>mountvol

建立、刪除或列出磁片區掛接點。 您也可以連結磁片區，而不需要磁碟機號。

## <a name="syntax"></a>語法

```
mountvol [<drive>:]<path volumename>
mountvol [<drive>:]<path> /d
mountvol [<drive>:]<path> /l
mountvol [<drive>:]<path> /p
mountvol /r
mountvol [/n|/e]
mountvol <drive>: /s
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `[<drive>:]<path>` | 指定掛接點所在的現有 NTFS 目錄。 |
| `<volumename>` | 指定磁片區名稱，此為掛接點的目標。 磁片區名稱會使用下列語法，其中*GUID*是全域唯一識別碼： `\\?\volume\{GUID}\` 。 需要括弧 `{ }` 。 |
| /d | 移除指定資料夾中的磁片區掛接點。 |
| /l | 列出所指定資料夾的掛接磁片區名稱。 |
| /p | 移除指定目錄中的磁片區掛接點、卸載基本磁碟區，然後讓基本磁碟區離線，使其無法安裝。 如果有其他進程正在使用此磁片區， **mountvol**會先關閉所有開啟的控制碼，再卸載磁片區。 |
| /r | 移除已不在系統中之磁片區的磁片區掛接點目錄和登錄設定，使其無法自動掛接，並在將其先前的磁片區掛接點新增回系統時，將其指定為 (s) 。 |
| /n | 停用新基本磁碟區的自動裝載。 新增至系統時，不會自動掛接新的磁片區。 |
| /e | 重新啟用新基本磁碟區的自動裝載。 |
| /s | 在指定的磁片磁碟機上裝載 EFI 系統磁碟分割。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- 如果您在使用 **/p**參數時卸載磁片區，則磁片區清單會將磁片區顯示為 [未裝載]，直到磁片區掛接點建立為止。

- 如果您的磁片區有多個掛接點，請在使用 **/p**之前，使用 **/d**移除額外的掛接點。 您可以藉由指派磁片區掛接點，讓基本磁碟區重新裝載。

- 如果您需要擴充磁片區空間而不重新格式化或更換硬碟，您可以將掛接路徑新增至另一個磁片區。 使用一個含有數個掛接路徑的磁片區的優點是，您可以使用單一磁碟機號 (（例如) ）來存取所有本機磁片區 `C:` 。 您不需要記住哪一個磁片區對應到哪個磁碟機號，雖然您仍然可以掛接本機磁片區，並將磁碟機號指派給它們。

## <a name="examples"></a>範例

若要建立掛接點，請輸入：

```
mountvol \sysmount \\?\volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
