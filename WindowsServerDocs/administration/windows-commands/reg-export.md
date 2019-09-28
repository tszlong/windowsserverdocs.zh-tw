---
title: reg 匯出
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fb3a779ffe5a4e7d513ca9a3afed8ee90901688
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384746"
---
# <a name="reg-export"></a>reg 匯出



將本機電腦的指定子機碼、專案和值複製到檔案中，以傳輸到其他伺服器。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Reg export KeyName FileName [/y]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName >|指定子機碼的完整路徑。 匯出操作只適用于本機電腦。 KeyName 必須包含有效的根金鑰。 有效的根金鑰如下：HKLM、HKCU、HKCR、HKU 和 HKCC。|
|\<檔案名 >|指定要在作業期間建立之檔案的名稱和路徑。 檔案的副檔名必須是 .reg。|
|/y|以名稱*FileName*覆寫任何現有的檔案，而不提示確認。|
|/?|在命令提示字元中顯示**reg export**的說明。|

## <a name="remarks"></a>備註

下表列出**reg 匯出**作業的傳回值。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="BKMK_examples"></a>典型

若要將金鑰 MyApp 的所有子機碼和值匯出至 AppBkUp 檔案，請輸入：
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)