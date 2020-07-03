---
title: reg save
description: Reg save 命令的參考文章，會將登錄的指定子機碼、專案和值的複本儲存在指定的檔案中。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4051d69819cfd3550d094de8e9d4bc73f77c4e4b
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931032"
---
# <a name="reg-save"></a>reg save

將登錄的指定子機碼、專案和值的複本儲存在指定的檔案中。

## <a name="syntax"></a>語法

```
reg save <keyname> <filename> [/y]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<keyname>` | 指定子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱（格式為）包含在 `\\<computername>\` *keyname*中。 省略 `\\<computername>\` 會使操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和**HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM**和**HKU**。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。 |
| `<filename>` | 指定所建立檔案的名稱和路徑。 如果未指定路徑，則會使用目前的路徑。 |
| /y | 覆寫名稱為*filename*的現有檔案，而不提示確認。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 在編輯任何登錄專案之前，您必須使用**reg save**命令來儲存父系子機碼。 如果編輯失敗，您可以接著使用**reg 還原**作業來還原原始的子機碼。

- **Reg 儲存**作業的傳回值如下：

    | 值 | 說明 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要將 hive MyApp 儲存至目前的資料夾，做為名為 AppBkUp 的檔案，請輸入：

```
reg save HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [reg restore 命令](reg-restore.md)