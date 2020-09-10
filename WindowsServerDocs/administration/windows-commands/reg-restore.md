---
title: reg restore
description: Reg restore 命令的參考文章，此命令會將已儲存的子機碼和專案寫入登錄。
ms.topic: reference
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d5c0e51ed3b16c2655647919f360a56b2aee6344
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639852"
---
# <a name="reg-restore"></a>reg restore

將已儲存的子機碼和專案寫回登錄。

## <a name="syntax"></a>語法

```
reg restore <keyname> <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname>` | 指定要還原之子機碼的完整路徑。 還原作業只適用于本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和 **HKCC**。 如果登錄機碼名稱包含空格，請將金鑰名稱括在引號中。 |
| `<filename>` | 指定檔案的名稱和路徑，以及要寫入登錄的內容。 您必須事先使用登錄 **儲存** 命令來建立這個檔案，而且必須使用 hiv 副檔名。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 在編輯任何登錄專案之前，您必須使用登錄 **儲存** 命令來儲存父子機碼。 如果編輯失敗，您就可以使用 **reg restore** 作業來還原原始子機碼。

- **Reg 還原**作業的傳回值為：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要將名為 NTRKBkUp. hiv 的檔案還原至金鑰 HKLM\Software\Microsoft\ResKit，並覆寫現有的金鑰內容，請輸入：

```
reg restore HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [reg save 命令](reg-save.md)
