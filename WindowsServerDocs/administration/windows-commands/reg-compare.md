---
title: reg compare
description: Reg compare 命令的參考文章，可比較指定的登錄子機碼或專案。
ms.topic: reference
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 51c385b9a051574602c2508b8efd8f657c62ec7c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627094"
---
# <a name="reg-compare"></a>reg compare

比較指定的登錄子機碼或專案。

## <a name="syntax"></a>語法

```
reg compare <keyname1> <keyname2> [{/v Valuename | /ve}] [{/oa | /od | /os | on}] [/s]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<keyname1>` | 指定要新增之子機碼或專案的完整路徑。 若要指定遠端電腦，請將電腦名稱稱 (以 `\\<computername>\`) 作為 *keyname*一部分的格式來包含。 省略 `\\<computername>\` 會導致操作預設為本機電腦。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和 **HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM** 和 **HKU**。 如果登錄機碼名稱包含空格，請將金鑰名稱括在引號中。 |
| `<keyname2>` | 指定要比較之第二個子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱 (以 `\\<computername>\`) 作為 *keyname*一部分的格式來包含。 省略 `\\<computername>\` 會導致操作預設為本機電腦。 在 *keyname2* 中只指定電腦名稱稱會導致作業使用 *keyname1*中所指定子機碼的路徑。 *Keyname*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： **HKLM**、 **HKCU**、 **HKCR**、 **HKU**和 **HKCC**。 如果指定遠端電腦，有效的根金鑰為： **HKLM** 和 **HKU**。 如果登錄機碼名稱包含空格，請將金鑰名稱括在引號中。 |
| /v `<Valuename>` | 指定要在子機碼下比較的值名稱。 |
| /ve | 指定只應比較值名稱為 null 的專案。 |
| /oa | 指定顯示所有差異和相符專案。 依預設，只會列出差異。 |
| /od | 指定只顯示差異。 此為預設行為。 |
| /os | 指定只顯示相符專案。 依預設，只會列出差異。 |
| /on | 指定不顯示任何內容。 依預設，只會列出差異。 |
| /s | 以遞迴方式比較所有子機碼和專案。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Reg compare**作業的傳回值如下：

    | 值 | 描述 |
    |--|--|
    | 0 | 比較成功，結果也相同。 |
    | 1 | 比較失敗。 |
    | 2 | 比較成功，併發現差異。 |

- 結果中顯示的符號包括：

    | 符號 | 描述 |
    |--|--|
    | = | *KeyName1* 資料等於 *KeyName2* 資料。 |
    | < | *KeyName1* 資料小於 *KeyName2* 的資料。 |
    | > | *KeyName1* 資料大於 *KeyName2* 的資料。 |

### <a name="examples"></a>範例

若要將機碼 **MyApp** 下的所有值與金鑰 **SaveMyApp**下的所有值進行比較，請輸入：

```
reg compare HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp
```

若要比較 [金鑰] **MyCo** 下的版本值和 [金鑰 **MyCo1**] 下的版本值，請輸入：

```
reg compare HKLM\Software\MyCo HKLM\Software\MyCo1 /v Version
```

若要比較名為天干的電腦上 HKLM\Software\MyCo 下的所有子機碼和值，並在本機電腦上以 HKLM\Software\MyCo 的所有子機碼和值進行比較，請輸入：

```
reg compare \\ZODIAC\HKLM\Software\MyCo \\. /s
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
