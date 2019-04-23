---
title: reg 匯出
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d7aeddb4b069b1baf5b8f7aaea2730a2b25bdad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889649"
---
# <a name="reg-export"></a>reg 匯出



將指定的子機碼、 項目，以及本機電腦的值到傳輸的檔案複製到其他伺服器。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Reg export KeyName FileName [/y]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName>|指定的子機碼的完整路徑。 匯出作業只適用於本機電腦。 KeyName 必須包含有效的根機碼。 有效的根機碼如下：HKLM、 HKCU、 HKCR、 HKU 和 HKCC。|
|\<FileName>|指定要在作業期間建立之檔案的路徑與名稱。 檔案必須具有.reg 延伸模組。|
|/y|覆寫任何現有的檔案同名*FileName*不提示確認。|
|/?|顯示的說明**reg 匯出**在命令提示字元。|

## <a name="remarks"></a>備註

下表列出的傳回值**reg 匯出**作業。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="BKMK_examples"></a>範例

若要將所有子機碼的內容和值的索引鍵的 MyApp 匯出至檔案 AppBkUp.reg 中，輸入：
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)