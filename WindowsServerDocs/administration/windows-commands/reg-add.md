---
title: 新增登錄
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61ed9273c0225477d8bd6043dfc599e3e3ae6228
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868879"
---
# <a name="reg-add"></a>新增登錄


將新的子機碼或項目加入至登錄中。

## <a name="syntax"></a>語法

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName*>*|指定要加入的項目之子機碼的完整路徑。 若要指定遠端電腦，包含電腦名稱 (格式\\ \\\<電腦名稱 >\)一部分*KeyName*。 省略\\ \\ComputerName\ 會導致作業到本機電腦的預設值。 *KeyName*必須包含有效的根機碼。 在本機電腦的有效的根機碼如下：HKLM、 HKCU、 HKCR、 HKU 和 HKCC。 如果指定的遠端電腦，則會是有效的根機碼：HKLM 和 HKU。 如果登錄機碼名稱包含空格，請用引號括住索引鍵的名稱。|
|/v \<ValueName>|指定要加入指定的子機碼下的登錄項目名稱。|
|/ve|指定新增至登錄的登錄項目具有 null 值。|
|/t \<Type>|指定的登錄項目類型。 *型別*必須是下列其中之一：</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br>REG_EXPAND_SZ|
|/s \<Separator>|指定要用來分隔多個資料執行個體時指定 REG_MULTI_SZ 資料類型和多個項目必須先列的字元。 如果未指定，預設的分隔符號是**\0**。|
|/d\<資料 >|指定新的登錄項目的資料。|
|/f|新增登錄項目，而不提示確認。|
|/?|顯示的說明**reg 新增**在命令提示字元。|

## <a name="remarks"></a>備註

-   無法新增子樹，這項作業。 本版**reg**不會要求確認時新增子機碼。
-   下表列出的傳回值**reg 新增**作業。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|
-   REG_EXPAND_SZ 索引鍵類型使用插入號符號 ( **^** ) 與**%**「 內部 /d 參數

## <a name="BKMK_examples"></a>範例

若要新增的索引鍵 HKLM\Software\MyCo ABC 的遠端電腦上，請輸入：
```
REG ADD \\ABC\HKLM\Software\MyCo
```
登錄項目加入名為值的 HKLM\Software\MyCo**資料**的 REG_BINARY 和資料類型**fe340ead**，型別：
```
REG ADD HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```
若要將多重值的登錄項目加入值名稱為 HKLM\Software\MyCo **MRU**的 REG_MULTI_SZ 和資料類型**fax\0mail\0\0**，型別：
```
REG ADD HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```
若要將展開的登錄項目加入值名稱為 HKLM\Software\MyCo**路徑**的輸入 REG_EXPAND_SZ 和的資料 **%systemroot%**，型別：
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
