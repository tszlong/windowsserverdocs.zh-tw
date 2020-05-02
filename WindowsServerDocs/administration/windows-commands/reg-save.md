---
title: reg 儲存
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dd3e932e67df7eb972bd625ecec24f986cf3f3d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722506"
---
# <a name="reg-save"></a>reg 儲存



將登錄的指定子機碼、專案和值的複本儲存在指定的檔案中。



## <a name="syntax"></a>語法

```
reg save <KeyName> <FileName> [/y]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName>|指定子機碼的完整路徑。 若要指定遠端電腦，請包含電腦名稱稱\\ \\（格式為 ComputerName\) *的一部分）。* 省略\\ \\ComputerName \ 會使此操作預設為本機電腦。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰為： HKLM 和 HKU。|
|\<檔案名>|指定所建立之檔案的名稱和路徑。 如果未指定路徑，則會使用目前的路徑。|
|/y|覆寫名稱為*FileName*的現有檔案，而不提示確認。|
|/?|在命令提示字元中顯示**reg save**的說明。|

## <a name="remarks-optional-section"></a>備註\<選擇性區段>

-   下表列出**reg 儲存**作業的傳回值。

|值|描述|
|-----|-----------|
|0|Success|
|1|失敗|
-   在編輯任何登錄專案之前，請使用**reg save**作業來儲存父系子機碼。 如果編輯失敗，請使用**reg restore**作業來還原原始的子機碼。

## <a name="examples"></a>範例

若要將 hive MyApp 儲存至目前的資料夾，做為名為 AppBkUp 的檔案，請輸入：
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)