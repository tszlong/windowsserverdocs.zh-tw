---
title: reg 匯入
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d1dd1b61848671b528c62fd22fe656e14fda7b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861839"
---
# <a name="reg-import"></a>reg 匯入



包含的檔案的內容複製到本機電腦的登錄，匯出登錄子機碼、 項目，以及值。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Reg import FileName
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<FileName>|指定包含要複製到本機電腦上的內容檔案的路徑與名稱。 必須事先建立此檔案，使用**reg 匯出**。|
|/?|顯示的說明**reg 匯入**在命令提示字元。|

## <a name="remarks"></a>備註

下表列出的傳回值**reg 匯入**作業。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="BKMK_examples"></a>範例

若要從名為 AppBkUp.reg 檔案，匯入登錄項目，請輸入：
```
reg import AppBkUp.reg
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)