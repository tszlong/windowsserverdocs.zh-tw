---
title: reg copy
description: Reg copy 命令的參考文章，它會將登錄專案複製到本機或遠端電腦上的指定位置。
ms.topic: article
ms.assetid: 3fe74213-39ec-4b2d-ba3d-086243eac997
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc1141ddd8082ee6302886a5ce49b9805a19cede
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884181"
---
# <a name="reg-copy"></a>reg copy

將登錄專案複製到本機或遠端電腦上的指定位置。

## <a name="syntax"></a>語法

```
reg copy <keyname1> <keyname2> [/s] [/f]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname1>` | 指定要新增之子機碼或專案的完整路徑。 若要指定遠端電腦，請將電腦名稱稱的格式加上 `\\<computername>\`) 作為*keyname*的一部分 (。 省略 `\\<computername>\` 會使操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和**HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM**和**HKU**。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。 |
| `<keyname2>` | 指定要比較之第二個子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱的格式加上 `\\<computername>\`) 作為*keyname*的一部分 (。 省略 `\\<computername>\` 會使操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和**HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM**和**HKU**。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。 |
| /s | 複製指定子機碼下的所有子機碼和專案。 |
| /f | 複製子機碼，而不提示確認。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 複製子機碼時，此命令不會要求確認。

- **Reg compare**作業的傳回值如下：

    | 值 | 描述 |
    |--|--|
    | 0 | 成功 |
    | 1 | 失敗 |

### <a name="examples"></a>範例

若要將金鑰 MyApp 底下的所有子機碼和值複製到金鑰 SaveMyApp，請輸入：

```
reg copy HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```

若要將名稱為天干地支之電腦機碼 MyCo 下的所有值複製到目前電腦上的金鑰 MyCo1，請輸入：

```
reg copy \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
