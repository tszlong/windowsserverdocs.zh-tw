---
title: reg 匯入
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e7e033091752f97086fd27fcb94e62469f0cced
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722546"
---
# <a name="reg-import"></a>reg 匯入



將包含已匯出登錄子機碼、專案和值的檔案內容複寫到本機電腦的登錄中。



## <a name="syntax"></a>語法

```
Reg import FileName
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<檔案名>|指定檔案的名稱和路徑，該檔案包含要複製到本機電腦之登錄中的內容。 必須事先使用**reg export**建立此檔案。|
|/?|在命令提示字元中顯示**reg 匯入**的說明。|

## <a name="remarks"></a>備註

下表列出**reg 匯入**作業的傳回值。

|值|描述|
|-----|-----------|
|0|Success|
|1|失敗|

## <a name="examples"></a>範例

若要從名為 AppBkUp 的檔案匯入登錄專案，請輸入：
```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)