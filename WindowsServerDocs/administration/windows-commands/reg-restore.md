---
title: reg 還原
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0025a37ed8ca50b47e7750501a7362659b500537
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858769"
---
# <a name="reg-restore"></a>reg 還原



寫入儲存子機碼和項目回到登錄。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Reg restore <KeyName> <FileName>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName>|指定要還原的子機碼的完整路徑。 還原作業只適用於本機電腦。 KeyName 必須包含有效的根機碼。 有效的根機碼如下：HKLM、 HKCU、 HKCR、 HKU 和 HKCC。|
|\<FileName>|要寫入至登錄的內容中指定的名稱和檔案的路徑。 必須事先建立此檔案，與**reg 儲存**使用.hiv 延伸模組的作業。|
|/?|顯示的說明**reg 還原**在命令提示字元。|

## <a name="remarks"></a>備註

-   然後再編輯任何登錄項目，儲存與父代子機碼**reg 儲存**作業。 如果編輯失敗，還原與原始的子機碼**reg 還原**作業。
-   下表列出的傳回值**reg 還原**作業。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="BKMK_examples"></a>範例

若要還原名為 NTRKBkUp.hiv 轉換成金鑰 HKLM\Software\Microsoft\ResKit，該檔案，並覆寫現有的索引鍵內容，請輸入：
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)