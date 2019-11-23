---
title: reg 儲存
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6ae07cd3c90c51e7bd494bc6c35919680cde912a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371699"
---
# <a name="reg-save"></a>reg 儲存



將登錄的指定子機碼、專案和值的複本儲存在指定的檔案中。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
reg save <KeyName> <FileName> [/y]
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|\<KeyName >|指定子機碼的完整路徑。 若要指定遠端電腦，請包含電腦名稱稱（格式為 \\\\ComputerName\) 作為*KeyName*的一部分。 省略 \\\\ComputerName \ 會使操作預設為本機電腦。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰為： HKLM 和 HKU。|
|\<FileName >|指定所建立之檔案的名稱和路徑。 如果未指定路徑，則會使用目前的路徑。|
|/y|覆寫名稱為*FileName*的現有檔案，而不提示確認。|
|/?|在命令提示字元中顯示**reg save**的說明。|

## <a name="remarks-optional-section"></a>\<選擇性區段的備註 >

-   下表列出**reg 儲存**作業的傳回值。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|
-   在編輯任何登錄專案之前，請使用**reg save**作業來儲存父系子機碼。 如果編輯失敗，請使用**reg restore**作業來還原原始的子機碼。

## <a name="BKMK_examples"></a>典型

若要將 hive MyApp 儲存至目前的資料夾，做為名為 AppBkUp 的檔案，請輸入：
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)