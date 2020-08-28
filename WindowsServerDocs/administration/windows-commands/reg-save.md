---
title: reg save
description: 登錄儲存命令的參考文章，此命令會在指定的檔案中儲存指定的子機碼、專案及登錄值的複本。
ms.topic: reference
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17c1bd3439d98ee2e0aa64cb3000f94dfbab41f4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025002"
---
# <a name="reg-save"></a>reg save

將登錄的指定子機碼、專案和值的複本儲存在指定的檔案中。

## <a name="syntax"></a>語法

```
reg save <keyname> <filename> [/y]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname>` | 指定子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱 (以 `\\<computername>\`) 作為 *keyname*一部分的格式來包含。 省略 `\\<computername>\` 會導致操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和 **HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM** 和 **HKU**。 如果登錄機碼名稱包含空格，請將金鑰名稱括在引號中。 |
| `<filename>` | 指定所建立之檔案的名稱和路徑。 如果未指定路徑，則會使用目前的路徑。 |
| /y | 使用名稱 *檔案名* 覆寫現有的檔案，而不提示確認。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 在編輯任何登錄專案之前，您必須使用登錄 **儲存** 命令來儲存父子機碼。 如果編輯失敗，您就可以使用 **reg restore** 作業來還原原始子機碼。

- **Reg save**作業的傳回值為：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要將 hive MyApp 儲存在目前資料夾中，並將其命名為 AppBkUp. hiv，請輸入：

```
reg save HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [reg restore 命令](reg-restore.md)