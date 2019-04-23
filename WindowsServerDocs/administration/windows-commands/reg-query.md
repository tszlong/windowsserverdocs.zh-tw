---
title: reg 查詢
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e239184cc5d118a858d012528fd8135f0b834e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827749"
---
# <a name="reg-query"></a>reg 查詢



傳回下一層的子機碼和位於登錄中的指定子機碼下的項目清單。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName>|指定的子機碼的完整路徑。 指定遠端電腦，包含電腦名稱 (格式\\ \\ComputerName\)一部分*KeyName*。 省略\\ \\ComputerName\ 會導致作業到本機電腦的預設值。 *KeyName*必須包含有效的根機碼。 在本機電腦的有效的根機碼如下：HKLM、 HKCU、 HKCR、 HKU 和 HKCC。 如果指定的遠端電腦，則會是有效的根機碼：HKLM 和 HKU。|
|/v \<ValueName>|指定會查詢登錄值名稱。 如果省略，所有的值名稱*KeyName*會傳回。 *ValueName*這個參數是選擇性如果 **/f**選項也會使用。|
|/ve|執行查詢的值是空的名稱。|
|/s|指定要查詢所有的子機碼和值名稱以遞迴方式。|
|/se \<Separator>|指定要搜尋的值名稱 輸入 REG_MULTI_SZ 的單一值分隔符號。 如果*分隔符號*未指定，則**\0**用。|
|/f\<資料 >|指定要搜尋模式的資料。 如果字串包含空格，請使用雙引號括住。 如果未指定萬用字元 (**&#42;**) 做為搜尋模式。|
|/k|指定搜尋中只使用索引鍵的名稱。|
|/d|指定要在僅限資料中搜尋。|
|/c|指定查詢會區分大小寫。 根據預設，查詢不會區分大小寫。|
|/e|指定要傳回只有精確相符項目。 根據預設，會傳回所有相符項目。|
|/t \<Type>|指定要搜尋的登錄類型。 有效類型為：REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. 如果未指定，則會搜尋所有型別。|
|/z|指定要在搜尋結果中包含的登錄類型相當的數值。|
|/?|顯示的說明**reg 查詢**在命令提示字元。|

## <a name="remarks-optional-section"></a>備註\<選擇性區段 >

下表列出的傳回值**reg 查詢**作業。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="BKMK_examples"></a>範例

若要顯示的名稱值版本 HKLM\Software\Microsoft\ResKit 機碼中，輸入：
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
若要在名為 ABC 的遠端電腦上顯示所有的子機碼和機碼 HKLM\Software\Microsoft\ResKit\Nt\Setup 下的值，請輸入：
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
若要顯示的所有子機碼和值的型別使用 REG_MULTI_SZ **#** 做為分隔符號，請輸入：
```
REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```
若要顯示索引鍵，值和資料完全相符和區分大小寫比對系統的資料類型為 REG_SZ，型別 HKLM 根目錄下：
```
REG QUERY HKLM /f SYSTEM /t REG_SZ /c /e
```
若要顯示索引鍵、 值和相符的資料**0F**中的資料在 HKCU 根機碼下的資料類型 REG_BINARY。
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
若要顯示的值和資料值是 null （預設值） 底下 HKLM\SOFTWARE 的名稱，請輸入：
```
REG QUERY HKLM\SOFTWARE /ve
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)