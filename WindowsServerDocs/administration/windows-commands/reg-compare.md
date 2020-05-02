---
title: reg 比較
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 49e9b993f512fdbc4728ee08ec42a8bc7ce0ab8f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722593"
---
# <a name="reg-compare"></a>reg 比較



比較指定的登錄子機碼或專案。



## <a name="syntax"></a>語法

```
reg compare <KeyName1> <KeyName2> [{/v ValueName | /ve}] [{/oa | /od | /os | on}] [/s]
```

### <a name="parameters"></a>參數

|    參數    |                                                                                                                                                                                                                                                                                          說明                                                                                                                                                                                                                                                                                           |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<KeyName1>   |                                                               指定要比較之第一個子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱\\ \\（格式為\) *ComputerName 的一部分*）包含在內。 省略\\ \\ComputerName \ 會使此操作預設為本機電腦。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰為： HKLM 和 HKU。                                                                |
|   \<KeyName2>   | 指定要比較之第二個子機碼的完整路徑。 若要指定遠端電腦，請將電腦名稱稱\\ \\（格式為\) *ComputerName 的一部分*）包含在內。 省略\\ \\ComputerName \ 會使此操作預設為本機電腦。 只在*KeyName2*中指定電腦名稱稱，會使作業使用*KeyName1*中指定之子機碼的路徑。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰為： HKLM 和 HKU。 |
| /v \<ValueName> |                                                                                                                                                                                                                                                                     指定要在子機碼底下比較的值名稱。                                                                                                                                                                                                                                                                      |
|       /ve       |                                                                                                                                                                                                                                                         指定只應比較值名稱為 null 的專案。                                                                                                                                                                                                                                                         |
|      [{/oa      |                                                                                                                                                                                                                                                                                              /od                                                                                                                                                                                                                                                                                               |
|       /oa       |                                                                                                                                                                                                                                             指定顯示所有差異和相符專案。 根據預設，只會列出差異。                                                                                                                                                                                                                                             |
|       /od       |                                                                                                                                                                                                                                                          指定只顯示差異。 此為預設行為。                                                                                                                                                                                                                                                          |
|       /os       |                                                                                                                                                                                                                                                    指定只顯示相符專案。 根據預設，只會列出差異。                                                                                                                                                                                                                                                     |
|       /on       |                                                                                                                                                                                                                                                       指定不顯示任何內容。 根據預設，只會列出差異。                                                                                                                                                                                                                                                        |
|       /s        |                                                                                                                                                                                                                                                                         以遞迴方式比較所有子機碼和專案。                                                                                                                                                                                                                                                                          |
|       /?        |                                                                                                                                                                                                                                                                    在命令提示字元中顯示**reg 比較**的說明。                                                                                                                                                                                                                                                                    |

## <a name="remarks"></a>備註

下表列出**reg compare**的傳回值。

|值|描述|
|-----|-----------|
|0|比較成功，且結果完全相同。|
|1|比較失敗。|
|2|比較成功，而且發現差異。|

下表列出結果中顯示的符號。

|符號|描述|
|------|-----------|
|=|*KeyName1*資料等於*KeyName2*資料。|
|<|*KeyName1*資料小於*KeyName2*的資料。|
|>|*KeyName1*資料大於*KeyName2*資料。|

## <a name="examples"></a>範例

若要比較 key **MyApp**底下的所有值與機碼**SaveMyApp**下的所有值，請輸入：

REG COMPARE HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp

若要比較 [金鑰] **MyCo**下的 [版本] 值和 [金鑰] **MyCo1**下的 [版本] 值，請輸入：

REG COMPARE HKLM\Software\MyCo HKLM\Software\MyCo1/v 版本

若要比較電腦上 HKLM\Software\MyCo 的所有子機碼和值，以及本機電腦上 HKLM\Software\MyCo 下的所有子機碼和值，請輸入：

REG COMPARE \\ \\ZODIAC\HKLM\Software\MyCo \\ \\。 /s

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)