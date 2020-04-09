---
title: reg 還原
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e511694247c04f2befc9c0148498e43b85f664ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836361"
---
# <a name="reg-restore"></a>reg 還原



將已儲存的子機碼和專案寫入登錄。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Reg restore <KeyName> <FileName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName >|指定要還原之子機碼的完整路徑。 還原作業只適用于本機電腦。 KeyName 必須包含有效的根金鑰。 有效的根金鑰包括： HKLM、HKCU、HKCR、HKU 和 HKCC。|
|\<FileName >|指定包含要寫入登錄之內容的檔案名和路徑。 此檔案必須預先建立，並使用 hiv 副檔名來進行**reg save**作業。|
|/?|在命令提示字元中顯示**reg 還原**的說明。|

## <a name="remarks"></a>備註

-   在編輯任何登錄專案之前，請使用**reg save**作業來儲存父系子機碼。 如果編輯失敗，請使用**reg restore**作業來還原原始的子機碼。
-   下表列出**reg 還原**作業的傳回值。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要將名為 NTRKBkUp. hiv 的檔案還原到金鑰 HKLM\Software\Microsoft\ResKit 中，並覆寫現有的金鑰內容，請輸入：
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)