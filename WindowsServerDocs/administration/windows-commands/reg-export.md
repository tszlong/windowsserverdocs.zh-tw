---
title: reg 匯出
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5901014511fed0c17a641e1ed183ddbf40dd44c8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836491"
---
# <a name="reg-export"></a>reg 匯出



將本機電腦的指定子機碼、專案和值複製到檔案中，以傳輸到其他伺服器。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Reg export KeyName FileName [/y]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName >|指定子機碼的完整路徑。 匯出操作只適用于本機電腦。 KeyName 必須包含有效的根金鑰。 有效的根金鑰包括： HKLM、HKCU、HKCR、HKU 和 HKCC。|
|\<FileName >|指定要在作業期間建立之檔案的名稱和路徑。 檔案的副檔名必須是 .reg。|
|/y|以名稱*FileName*覆寫任何現有的檔案，而不提示確認。|
|/?|在命令提示字元中顯示**reg export**的說明。|

## <a name="remarks"></a>備註

下表列出**reg 匯出**作業的傳回值。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要將金鑰 MyApp 的所有子機碼和值匯出至 AppBkUp 檔案，請輸入：
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)