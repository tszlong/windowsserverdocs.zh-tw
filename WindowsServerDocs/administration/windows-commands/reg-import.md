---
title: reg 匯入
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0816297e837bbce91ca069e3506405cbdb53c51a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836421"
---
# <a name="reg-import"></a>reg 匯入



將包含已匯出登錄子機碼、專案和值的檔案內容複寫到本機電腦的登錄中。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Reg import FileName
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<FileName >|指定檔案的名稱和路徑，該檔案包含要複製到本機電腦之登錄中的內容。 必須事先使用**reg export**建立此檔案。|
|/?|在命令提示字元中顯示**reg 匯入**的說明。|

## <a name="remarks"></a>備註

下表列出**reg 匯入**作業的傳回值。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要從名為 AppBkUp 的檔案匯入登錄專案，請輸入：
```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)