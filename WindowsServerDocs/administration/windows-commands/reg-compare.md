---
title: reg compare
description: Reg compare 命令的參考文章，它會比較指定的登錄子機碼或專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b508a52d0f110455b09002a1044fefcf1be048a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930735"
---
# <a name="reg-compare"></a>reg compare

比較指定的登錄子機碼或專案。

## <a name="syntax"></a>語法

```
reg compare <keyname1> <keyname2> [{/v Valuename | /ve}] [{/oa | /od | /os | on}] [/s]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<keyname1>` | 指定要新增之子機碼或專案的完整路徑。 若要指定遠端電腦，請將電腦名稱稱（格式為）包含在 `\\<computername>\` *keyname*中。 省略 `\\<computername>\` 會使操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和**HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM**和**HKU**。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。 |
| `<keyname2>` | 指定要比較之第二個子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱（格式為）包含在 `\\<computername>\` *keyname*中。 省略 `\\<computername>\` 會使操作預設為本機電腦。 只在*keyname2*中指定電腦名稱稱，會使作業使用*keyname1*中指定之子機碼的路徑。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和**HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM**和**HKU**。 如果登錄機碼名稱包含空格，請用引號括住機碼名稱。 |
| 停`<Valuename>` | 指定要在子機碼底下比較的值名稱。 |
| /ve | 指定只應比較值名稱為 null 的專案。 |
| /oa | 指定顯示所有差異和相符專案。 根據預設，只會列出差異。 |
| /od | 指定只顯示差異。 此為預設行為。 |
| /os | 指定只顯示相符專案。 根據預設，只會列出差異。 |
| /on | 指定不顯示任何內容。 根據預設，只會列出差異。 |
| /s | 以遞迴方式比較所有子機碼和專案。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg compare**作業的傳回值如下：

    | 值 | 說明 |
    |--|--|
    | 0 | 比較成功，且結果完全相同。 |
    | 1 | 比較失敗。 |
    | 2 | 比較成功，而且發現差異。 |

- 結果中顯示的符號，包括：

    | 符號 | 描述 |
    |--|--|
    | = | *KeyName1*資料等於*KeyName2*資料。 |
    | < | *KeyName1*資料小於*KeyName2*的資料。 |
    | > | *KeyName1*資料大於*KeyName2*資料。 |

### <a name="examples"></a>範例

若要比較 key **MyApp**底下的所有值與機碼**SaveMyApp**下的所有值，請輸入：

```
reg compare HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp
```

若要比較 [金鑰] **MyCo**下的 [版本] 值和 [金鑰] **MyCo1**下的 [版本] 值，請輸入：

```
reg compare HKLM\Software\MyCo HKLM\Software\MyCo1 /v Version
```

若要比較電腦上 HKLM\Software\MyCo 的所有子機碼和值（名稱為天干//），並在本機電腦上的 HKLM\Software\MyCo 底下包含所有子機碼和值，請輸入：

```
reg compare \\ZODIAC\HKLM\Software\MyCo \\. /s
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
