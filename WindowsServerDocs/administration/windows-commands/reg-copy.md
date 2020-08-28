---
title: reg copy
description: Reg copy 命令的參考文章，此命令會將登錄專案複製到本機或遠端電腦上的指定位置。
ms.topic: reference
ms.assetid: 3fe74213-39ec-4b2d-ba3d-086243eac997
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a56f4d14c7dd52ba23f126c44ff940c694993377
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028066"
---
# <a name="reg-copy"></a>reg copy

將登錄專案複製到本機或遠端電腦上指定的位置。

## <a name="syntax"></a>語法

```
reg copy <keyname1> <keyname2> [/s] [/f]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname1>` | 指定要新增之子機碼或專案的完整路徑。 若要指定遠端電腦，請將電腦名稱稱 (以 `\\<computername>\`) 作為 *keyname*一部分的格式來包含。 省略 `\\<computername>\` 會導致操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和 **HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM** 和 **HKU**。 如果登錄機碼名稱包含空格，請將金鑰名稱括在引號中。 |
| `<keyname2>` | 指定要比較之第二個子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱 (以 `\\<computername>\`) 作為 *keyname*一部分的格式來包含。 省略 `\\<computername>\` 會導致操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和 **HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM** 和 **HKU**。 如果登錄機碼名稱包含空格，請將金鑰名稱括在引號中。 |
| /s | 複製指定子機碼下的所有子機碼和專案。 |
| /f | 複製子機碼，而不提示確認。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 此命令不會在複製子機碼時要求確認。

- **Reg compare**作業的傳回值如下：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要將金鑰 MyApp 下的所有子機碼和值複製到金鑰 SaveMyApp，請輸入：

```
reg copy HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```

若要將名為天干的電腦上的金鑰 MyCo 下的所有值複製到目前電腦上的機碼 MyCo1，請輸入：

```
reg copy \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
