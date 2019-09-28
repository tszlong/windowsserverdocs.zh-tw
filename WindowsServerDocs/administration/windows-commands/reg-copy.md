---
title: reg copy
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a82b17b631d4242fa6affdec0ff67b5b09380550
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371784"
---
# <a name="reg-copy"></a>reg copy



將登錄專案複製到本機或遠端電腦上的指定位置。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
reg copy <KeyName1> <KeyName2> [/s] [/f]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName1 >|指定要複製之子機碼的完整路徑。 若要指定遠端電腦，請包含電腦名稱稱（格式為 \\ @ no__t-1ComputerName @ no__t-2 作為*KeyName*的一部分。 省略 \\ @ no__t-1ComputerName \ 會使操作預設為本機電腦。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰如下：HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰如下：HKLM 和 HKU。|
|\<KeyName2 >|指定子機碼目的地的完整路徑。 若要指定遠端電腦，請包含電腦名稱稱（格式為 \\ @ no__t-1ComputerName @ no__t-2 作為*KeyName*的一部分。 省略 \\ @ no__t-1ComputerName \ 會使操作預設為本機電腦。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰如下：HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰如下：HKLM 和 HKU。|
|/s|複製指定子機碼下的所有子機碼和專案。|
|/f|複製子機碼，而不提示確認。|
|/?|在命令提示字元中顯示**reg** copy 的說明。|

## <a name="remarks"></a>備註

-   Reg 不會在複製子機碼時要求確認。
-   下表列出**reg copy**作業的傳回值。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="BKMK_examples"></a>典型

若要將金鑰 MyApp 底下的所有子機碼和值複製到金鑰 SaveMyApp，請輸入：
```
REG COPY HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```
若要將名稱為天干地支之電腦機碼 MyCo 下的所有值複製到目前電腦上的金鑰 MyCo1，請輸入：
```
REG COPY \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)