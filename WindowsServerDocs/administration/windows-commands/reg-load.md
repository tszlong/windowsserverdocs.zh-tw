---
title: reg load
description: Reg load 命令的參考文章，此命令會將已儲存的子機碼和專案寫入登錄中的不同子機碼。
ms.topic: reference
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8647b417999459b210986187bd523b3953a8b2d6
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028026"
---
# <a name="reg-load"></a>reg load

將已儲存的子機碼和專案寫入登錄中的不同子機碼。 此命令適用于用來疑難排解或編輯登錄專案的暫存檔案。

## <a name="syntax"></a>語法

```
reg load <keyname> <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname>` | 指定要載入之子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱 (以 `\\<computername>\`) 作為 *keyname*一部分的格式來包含。 省略 `\\<computername>\` 會導致操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和 **HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM** 和 **HKU**。 如果登錄機碼名稱包含空格，請將金鑰名稱括在引號中。  |
| `<filename>` | 指定要載入之檔案的名稱和路徑。 您必須事先使用登錄 **儲存** 命令來建立這個檔案，而且必須使用 hiv 副檔名。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg 載入**作業的傳回值為：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要將名為 TempHive. hiv 的檔案載入至機碼 HKLM\TempHive，請輸入：

```
reg load HKLM\TempHive TempHive.hiv
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [reg save 命令](reg-save.md)
