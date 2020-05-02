---
title: reg 載入
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2523876e2ea2305ede3289226c934c9352825ba9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722542"
---
# <a name="reg-load"></a>reg 載入



將已儲存的子機碼和專案寫入登錄中的不同子機碼。 適用于用來疑難排解或編輯登錄專案的暫存檔案。



## <a name="syntax"></a>語法

```
reg load KeyName FileName
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName>|指定要載入之子機碼的完整路徑。 若要指定遠端電腦，請包含電腦名稱稱\\ \\（格式為 ComputerName\) *的一部分）。* 省略\\ \\ComputerName \ 會使此操作預設為本機電腦。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰為： HKLM 和 HKU。|
|\<檔案名>|指定要載入之檔案的名稱和路徑。 這個檔案必須事先使用**reg save**作業和 hiv 副檔名來建立。|
|/?|在命令提示字元中顯示**reg 載入**的說明。|

## <a name="remarks"></a>備註

下表列出**reg 載入**作業的傳回值。

|值|描述|
|-----|-----------|
|0|Success|
|1|失敗|

## <a name="examples"></a>範例

若要將名為 TempHive. hiv 的檔案載入至金鑰 HKLM\TempHive，請輸入：
```
REG LOAD HKLM\TempHive TempHive.hiv
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)