---
title: 取代
description: Replace 命令的參考文章，可以取代現有的檔案，或將新檔案新增至目錄。
ms.topic: reference
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 44ece657b87b61bc9be6333644d05b8201061014
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626983"
---
# <a name="replace"></a>取代

取代目錄中現有的檔案。 如果搭配 **/a** 選項使用，此命令會將新檔案新增至目錄，而不是取代現有的檔案。

## <a name="syntax"></a>語法

```
replace [<drive1>:][<path1>]<filename> [<drive2>:][<path2>] [/a] [/p] [/r] [/w]
replace [<drive1>:][<path1>]<filename> [<drive2>:][<path2>] [/p] [/r] [/s] [/w] [/u]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[<drive1>:][<path1>]<filename>` | 指定來源檔案或檔案集的位置和名稱。 *Filename*選項是必要的，而且可以包含萬用字元 (**&#42;** 和 **？**) 。 |
| `[<drive2>:][<path2>]` | 指定目的地檔案的位置。 您無法為所取代的檔案指定檔案名。 如果您未指定磁片磁碟機或路徑，此命令會使用目前的磁片磁碟機和目錄作為目的地。 |
| /a | 將新檔案新增至目的地目錄，而不是取代現有的檔案。 您無法將這個命令列選項與 **/s** 或 **/u** 命令列選項搭配使用。 |
| /p | 在取代目的地檔案或新增原始程式檔之前，提示您確認。 |
| /r | 取代唯讀和未受保護的檔案。 如果您嘗試取代唯讀檔案，但未指定 **/r**，則會產生錯誤，並停止取代作業。 |
| /W | 等候您在開始搜尋來源檔案之前插入磁片。 如果您未指定 **/w**，此命令會在您按下 enter 之後，立即開始取代或新增檔案。 |
| /s | 搜尋目的地目錄中的所有子目錄，並取代相符的檔案。 您無法使用 **/s** 搭配 **/a** 命令列選項。 此命令不會搜尋在 *Path1*中指定的子目錄。 |
| /U | 只取代目的地目錄中的檔案，這些檔案早于來原始目錄中的檔案。 您無法搭配使用 **/u** 與 **/a** 命令列選項。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 當這個命令新增或取代檔案時，檔案名會顯示在螢幕上。 完成此命令後，會以下列其中一種格式顯示摘要行：

  ```
  nnn files added
  nnn files replaced
  no file added
  no file replaced
  ```

- 如果您使用的是磁片磁碟機，而且需要在執行此命令時切換磁片，您可以指定 **/w** 命令列選項，讓此命令等候您切換磁片。

- 您無法使用此命令來更新隱藏的檔案或系統檔案。

- 下表顯示每個結束代碼和其意義的簡短描述：

  | 結束碼 | 描述 |
  |--|--|
  | 0 | 此命令已成功取代或新增檔案。 |
  | 1 | 此命令遇到不正確的 MS-DOS 版本。 |
  | 2 | 此命令找不到來源檔案。 |
  | 3 | 此命令找不到來源或目的地路徑。 |
  | 5 | 使用者沒有您想要取代之檔案的存取權。 |
  | 8 | 系統記憶體不足，無法執行命令。 |
  | 11 | 使用者在命令列上使用了錯誤的語法。 |

> [!NOTE]
> 您可以在 batch 程式的 **if** 命令列上使用 ERRORLEVEL 參數，以處理此命令所傳回的結束代碼。

## <a name="examples"></a>範例

若要 *更新名為* (的檔案的所有版本，這會出現在磁片磁碟機 C： ) *上的多* 個目錄中，並在磁片磁碟機 a：的磁片磁碟機中使用最新版本的 node.js 檔案，輸入：

```
replace a:\phones.cli c:\ /s
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
