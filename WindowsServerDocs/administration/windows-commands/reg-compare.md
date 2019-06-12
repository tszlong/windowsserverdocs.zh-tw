---
title: reg 比較
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83bb010af4bfbf38ce41001d6a6001d5a3996090
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441926"
---
# <a name="reg-compare"></a>reg 比較



比較指定的登錄子機碼或項目。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
reg compare <KeyName1> <KeyName2> [{/v ValueName | /ve}] [{/oa | /od | /os | on}] [/s]
```

## <a name="parameters"></a>參數

|    參數    |                                                                                                                                                                                                                                                                                          描述                                                                                                                                                                                                                                                                                           |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<KeyName1>   |                                                               指定要比較的第一個子機碼的完整路徑。 若要指定遠端電腦，包含電腦名稱 (格式\\ \\ComputerName\)一部分*KeyName*。 省略\\ \\ComputerName\ 會導致作業到本機電腦的預設值。 *KeyName*必須包含有效的根機碼。 在本機電腦的有效的根機碼如下：HKLM、 HKCU、 HKCR、 HKU 和 HKCC。 如果指定的遠端電腦，則會是有效的根機碼：HKLM 和 HKU。                                                                |
|   \<KeyName2>   | 指定要比較的第二個子機碼的完整路徑。 若要指定遠端電腦，包含電腦名稱 (格式\\ \\ComputerName\)一部分*KeyName*。 省略\\ \\ComputerName\ 會導致作業到本機電腦的預設值。 指定只有中的電腦名稱*KeyName2*會導致使用的路徑中指定的子機碼的作業*KeyName1*。 *KeyName*必須包含有效的根機碼。 在本機電腦的有效的根機碼如下：HKLM、 HKCU、 HKCR、 HKU 和 HKCC。 如果指定的遠端電腦，則會是有效的根機碼：HKLM 和 HKU。 |
| /v \<ValueName> |                                                                                                                                                                                                                                                                     指定要比較子機碼下的值名稱。                                                                                                                                                                                                                                                                      |
|       /ve       |                                                                                                                                                                                                                                                         指定應該比較值的名稱是 null 的唯一項目。                                                                                                                                                                                                                                                         |
|      [{/oa      |                                                                                                                                                                                                                                                                                              /od                                                                                                                                                                                                                                                                                               |
|       /oa       |                                                                                                                                                                                                                                             指定要顯示所有的差異和相符項目。 根據預設，會列出的差異。                                                                                                                                                                                                                                             |
|       /od       |                                                                                                                                                                                                                                                          指定要顯示只有差異。 這是是預設行為。                                                                                                                                                                                                                                                          |
|       /os       |                                                                                                                                                                                                                                                    指定唯一的符合項目會顯示。 根據預設，會列出的差異。                                                                                                                                                                                                                                                     |
|       /on       |                                                                                                                                                                                                                                                       指定不顯示任何內容。 根據預設，會列出的差異。                                                                                                                                                                                                                                                        |
|       /s        |                                                                                                                                                                                                                                                                         比較所有子機碼和項目以遞迴方式。                                                                                                                                                                                                                                                                          |
|       /?        |                                                                                                                                                                                                                                                                    顯示的說明**reg 比較**在命令提示字元。                                                                                                                                                                                                                                                                    |

## <a name="remarks"></a>備註

下表列出的傳回值**reg 比較**。

|值|描述|
|-----|-----------|
|0|比較成功，而且結果完全相同。|
|1|比較失敗。|
|2|比較成功，而且找不到的差異。|

下表列出結果中顯示的符號。

|符號|描述|
|------|-----------|
|=|*KeyName1*的資料等於*KeyName2*資料。|
|<|*KeyName1*資料是小於*KeyName2*資料。|
|>|*KeyName1*資料大於*KeyName2*資料。|

## <a name="BKMK_examples"></a>範例

要比較的機碼下的所有值**MyApp**機碼下的所有值**SaveMyApp**，型別：

REG COMPARE HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp

要比較值的版本機碼下**MyCo**和機碼下的版本值**MyCo1**，型別：

REG COMPARE HKLM\Software\MyCo HKLM\Software\MyCo1 /v Version

若要比較所有的子機碼和下 HKLM\Software\MyCo 名 ZODIAC 為所有子機碼和值 HKLM\Software\MyCo 在本機電腦上的電腦上的值，請輸入：

REG COMPARE \\\\ZODIAC\HKLM\Software\MyCo \\\\. /s

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)