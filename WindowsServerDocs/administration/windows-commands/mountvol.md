---
title: mountvol
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03e7cefc7c7a00338972fc365b7c25d9c795c83e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437286"
---
# <a name="mountvol"></a>mountvol



建立、 刪除或列出的磁碟區掛接點。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
mountvol [<Drive>:]<Path VolumeName>
mountvol [<Drive>:]<Path> /d
mountvol [<Drive>:]<Path> /l
mountvol [<Drive>:]<Path> /p
mountvol /r
mountvol [/n | /e]
mountvol <Drive>: /s
```

## <a name="parameters"></a>參數

|     參數     |                                                                                                                           描述                                                                                                                            |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:]<Path> |                                                                                             指定現有的 NTFS 目錄掛接點所在的位置。                                                                                             |
|   \<VolumeName>   |                     指定做為目標的掛接點磁碟區名稱。 磁碟區名稱會使用下列語法，其中*GUID*是全域唯一識別碼：</br>`\\\\?\Volume\{GUID}\`</br>需要括號 {}。                      |
|        /d         |                                                                                                    移除指定的資料夾中的磁碟區掛接點。                                                                                                     |
|        /l         |                                                                                                     列出指定的資料夾已掛接的磁碟區名稱。                                                                                                      |
|        /p         | 從指定的目錄中移除磁碟區掛接點，卸載基本磁碟區，並讓基本磁碟區離線，使它無法掛接。 如果其他處理序正在使用的磁碟區**mountvol**卸載磁碟區之前關閉任何開啟的控制代碼。 |
|        /r         |             移除磁碟區掛接點目錄和磁碟區，便無法再在系統中，防止自動掛接，並指定其先前的磁碟區掛接點加回系統時的登錄設定。              |
|        /n         |                                                                      停用自動掛接新的基本磁碟區。 新增至系統時，不會自動掛接新磁碟區。                                                                       |
|        /e         |                                                                                                       重新啟用自動掛接新的基本磁碟區。                                                                                                        |
|        /s         |                                                                                在指定的磁碟機上掛載 EFI 系統磁碟分割。 適用於 Itanium 型電腦。                                                                                |
|        /?         |                                                                                                               在命令提示字元顯示說明。                                                                                                               |

## <a name="remarks"></a>備註

-   **Mountvol**可讓您連結而不需要磁碟機代號的磁碟區。
-   卸載所使用的磁碟區 **/p**列為磁碟區清單中 「 不裝載 UNTIL 的磁碟區掛接點已建立 」。 如果磁碟區有多個掛接點，使用 **/d**移除其他掛接點，再使用 **/p**。 您可以進行的基本磁碟區掛接再藉由指派的磁碟區掛接點。
-   如果您需要展開磁碟區空間，而不需要重新格式化或更換硬碟，您可以加入另一個磁碟區來掛接路徑。 使用數個的掛接路徑中的一個磁碟區的優點是可以使用單一磁碟機代號，來存取所有的本機磁碟區 (例如`C:`)。 您不需要記住哪一個磁碟區對應到哪個磁碟機代號，雖然您仍然可以掛接本機的磁碟區，並將其指派磁碟機代號。

## <a name="BKMK_examples"></a>範例

若要建立掛接點，請輸入：
```
mountvol \sysmount \\?\Volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)