---
title: 儲存登錄
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a46dfe081421ed727bd7ffeeab364e6c23dd801
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841079"
---
# <a name="reg-save"></a>儲存登錄



指定的檔案中儲存一份指定的子機碼、 項目，以及登錄的值。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
reg save <KeyName> <FileName> [/y]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName>|指定的子機碼的完整路徑。 指定遠端電腦，包含電腦名稱 (格式\\ \\ComputerName\)一部分*KeyName*。 省略\\ \\ComputerName\ 會導致作業到本機電腦的預設值。 *KeyName*必須包含有效的根機碼。 在本機電腦的有效的根機碼如下：HKLM、 HKCU、 HKCR、 HKU 和 HKCC。 如果指定的遠端電腦，則會是有效的根機碼：HKLM 和 HKU。|
|\<FileName>|指定建立的檔案路徑與名稱。 如果未指定路徑，則會使用目前的路徑。|
|/y|覆寫現有的檔案同名*FileName*不提示確認。|
|/?|顯示的說明**reg 儲存**在命令提示字元。|

## <a name="remarks-optional-section"></a>備註\<選擇性區段 >

-   下表列出的傳回值**reg 儲存**作業。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|
-   然後再編輯任何登錄項目，儲存與父代子機碼**reg 儲存**作業。 如果編輯失敗，還原與原始的子機碼**reg 還原**作業。

## <a name="BKMK_examples"></a>範例

若要為檔案命名為 AppBkUp.hiv 儲存 hive MyApp 到目前的資料夾，請輸入：
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)