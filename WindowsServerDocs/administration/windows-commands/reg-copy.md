---
title: reg 複製
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3fe74213-39ec-4b2d-ba3d-086243eac997
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11cbfce39433ea18632bec16c524da191def2a07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867559"
---
# <a name="reg-copy"></a>reg 複製



將登錄項目複製到本機或遠端電腦上指定的位置中。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
reg copy <KeyName1> <KeyName2> [/s] [/f]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName1>|指定要複製的子機碼的完整路徑。 若要指定遠端電腦，包含電腦名稱 (格式\\ \\ComputerName\)一部分*KeyName*。 省略\\ \\ComputerName\ 會導致作業到本機電腦的預設值。 *KeyName*必須包含有效的根機碼。 在本機電腦的有效的根機碼如下：HKLM、 HKCU、 HKCR、 HKU 和 HKCC。 如果指定的遠端電腦，則會是有效的根機碼：HKLM 和 HKU。|
|\<KeyName2>|指定子機碼的目的地的完整路徑。 若要指定遠端電腦，包含電腦名稱 (格式\\ \\ComputerName\)一部分*KeyName*。 省略\\ \\ComputerName\ 會導致作業到本機電腦的預設值。 *KeyName*必須包含有效的根機碼。 在本機電腦的有效的根機碼如下：HKLM、 HKCU、 HKCR、 HKU 和 HKCC。 如果指定的遠端電腦，則會是有效的根機碼：HKLM 和 HKU。|
|/s|複製所有的子機碼和指定的子機碼下的項目。|
|/f|複製子機碼，而不提示確認。|
|/?|顯示的說明**reg**複製在命令提示字元。|

## <a name="remarks"></a>備註

-   Reg 不會複製子機碼時要求確認。
-   下表列出的傳回值**reg 複製**作業。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="BKMK_examples"></a>範例

若要將所有的子機碼和機碼 MyApp 下的值複製到 SaveMyApp 的索引鍵中，輸入：
```
REG COPY HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```
若要複製在電腦上稱為 ZODIAC 至目前的電腦上的索引鍵 MyCo1 MyCo 機碼下的所有值，請輸入：
```
REG COPY \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)