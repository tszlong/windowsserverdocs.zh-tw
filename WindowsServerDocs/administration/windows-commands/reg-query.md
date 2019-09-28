---
title: reg 查詢
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d2f616fb33974df4327c7b2536b3143b75d116be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371725"
---
# <a name="reg-query"></a>reg 查詢



傳回位於登錄中指定子機碼下的下一層子機碼和專案的清單。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName >|指定子機碼的完整路徑。 若要指定遠端電腦，請包含電腦名稱稱（格式為 \\ @ no__t-1ComputerName @ no__t-2 作為*KeyName*的一部分。 省略 \\ @ no__t-1ComputerName \ 會使操作預設為本機電腦。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰如下：HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰如下：HKLM 和 HKU。|
|/v \<ValueName >|指定要查詢的登錄值名稱。 如果省略，則會傳回*KeyName*的所有值名稱。 如果也使用 **/f**選項，此參數的*ValueName*是選擇性的。|
|/ve|針對空白的值名稱執行查詢。|
|/s|指定以遞迴方式查詢所有子機碼和值名稱。|
|/se \<Separator >|指定要在值名稱類型 REG_MULTI_SZ 中搜尋的單一值分隔符號。 如果未指定*Separator* ，則會使用 **\ 0** 。|
|/f \<Data >|指定要搜尋的資料或模式。 如果字串包含空格，請使用雙引號。 如果未指定，則會使用 **&#42;** 萬用字元（）做為搜尋模式。|
|/k|指定只搜尋索引鍵名稱。|
|/d|指定只搜尋資料。|
|/c|指定查詢區分大小寫。 根據預設，查詢不會區分大小寫。|
|/e|指定只傳回完全相符的專案。 根據預設，會傳回所有相符專案。|
|/t \<Type >|指定要搜尋的登錄類型。 有效類型為：REG_SZ、REG_MULTI_SZ、REG_EXPAND_SZ、REG_DWORD、REG_BINARY、REG_NONE。 如果未指定，則會搜尋所有類型。|
|/z|指定在搜尋結果中包含登錄類型的對等數值。|
|/?|在命令提示字元中顯示**reg 查詢**的說明。|

## <a name="remarks-optional-section"></a>備註 \<optional 區段 >

下表列出**reg 查詢**作業的傳回值。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="BKMK_examples"></a>典型

若要在 HKLM\Software\Microsoft\ResKit 索引鍵中顯示 name 值版本的值，請輸入：
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
若要在名為 ABC 的遠端電腦上，顯示金鑰 HKLM\Software\Microsoft\ResKit\Nt\Setup 底下的所有子機碼和值，請輸入：
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
若要使用 **#** 做為分隔符號來顯示類型為 REG_MULTI_SZ 的所有子機碼和值，請輸入：
```
REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```
若要在資料類型為 REG_SZ 的 HKLM 根目錄下，顯示系統的確切和區分大小寫相符的索引鍵、值和資料，請輸入：
```
REG QUERY HKLM /f SYSTEM /t REG_SZ /c /e
```
顯示索引鍵、值和資料，其符合資料類型 REG_BINARY 之 HKCU 根機碼下資料中的**0F** 。
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
若要顯示 HKLM\SOFTWARE 下值名稱為 null （預設）的值和資料，請輸入：
```
REG QUERY HKLM\SOFTWARE /ve
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)