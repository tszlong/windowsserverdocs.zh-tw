---
title: mountvol
description: Mountvol 命令的參考文章，可建立、刪除或列出磁片區掛接點。
ms.topic: reference
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4da7562bd50072dc91538bd08b5462222857830d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640133"
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
| `<volumename>` | 指定掛接點目標的磁片區名稱。 磁片區名稱使用下列語法，其中 *GUID* 是全域唯一識別碼： `\\?\volume\{GUID}\` 。 需要括弧 `{ }` 。 |
| /d | 從指定的資料夾移除磁片區掛接點。 |
| /l | 列出指定之資料夾的載入磁片區名稱。 |
| /p | 從指定的目錄移除磁片區掛接點、卸載基本磁碟區，並讓基本磁碟區離線，使其無法使用。 如果有其他處理常式正在使用磁片區， **mountvol** 會在卸載磁片區之前關閉任何開啟的控制碼。 |
| /r | 移除已不在系統中之磁片區的磁片區掛接點目錄和登錄設定，防止它們自動掛接，並在將其先前的磁片區掛接點 (s 新增回系統時) 。 |
| /n | 停用新基本磁碟區的自動裝載。 新增至系統時，不會自動裝載新的磁片區。 |
| /e | 重新啟用新基本磁碟區的自動裝載。 |
| /s | 在指定的磁片磁碟機上裝載 EFI 系統磁碟分割。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- 如果您在使用 **/p** 參數時卸載磁片區，磁片區清單會將磁片區顯示為未裝載，直到建立磁片區掛接點為止。

- 如果您的磁片區有多個掛接點，請在使用 **/p**之前，先使用 **/d**移除其他掛接點。 您可以藉由指派磁片區掛接點，讓基本磁碟區再次安裝。

- 如果您需要在不重新格式化或更換硬碟的情況下擴充磁片區空間，您可以將掛接路徑新增至另一個磁片區。 使用一個磁片區搭配數個掛接路徑的優點是，您可以使用單一磁碟機號 (（例如) ）來存取所有本機磁片區 `C:` 。 您不需要記住哪個磁片區對應到哪個磁碟機號，不過您仍然可以掛接本機磁片區並指派磁碟機號。

## <a name="examples"></a>範例

若要建立掛接點，請輸入：

```
mountvol \sysmount \\?\volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
