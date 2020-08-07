---
title: reg restore
description: Reg restore 命令的參考文章，其會將已儲存的子機碼和專案寫入登錄。
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9642c0973968b3092f6f988017e8c4ad1ef16b09
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884023"
---
# <a name="reg-restore"></a>reg restore

將已儲存的子機碼和專案寫入登錄。

## <a name="syntax"></a>語法

```
reg restore <keyname> <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname>` | 指定要還原之子機碼的完整路徑。 還原作業只適用于本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和**HKCC**。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。 |
| `<filename>` | 指定包含要寫入登錄之內容的檔案名和路徑。 此檔案必須使用**reg save**命令事先建立，而且必須具有 hiv 副檔名。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 在編輯任何登錄專案之前，您必須使用**reg save**命令來儲存父系子機碼。 如果編輯失敗，您可以接著使用**reg 還原**作業來還原原始的子機碼。

- **Reg 還原**作業的傳回值如下：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要將名為 NTRKBkUp. hiv 的檔案還原到金鑰 HKLM\Software\Microsoft\ResKit 中，並覆寫現有的金鑰內容，請輸入：

```
reg restore HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [reg save 命令](reg-save.md)
