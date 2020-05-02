---
title: pushprinterconnections
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94ba2d09a3e67248a393350e46e971aa8b24b00d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722760"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



從群組原則讀取已部署的印表機連線設定，並視需要部署/移除印表機連接。

## <a name="syntax"></a>語法

```
pushprinterconnections <-log> <-?>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|<-記錄>|將每個使用者的 debug 記錄檔寫入% temp，或將每個機器的 debug 記錄檔寫入%windir%\temp|
|<-？ >|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

此公用程式可用於電腦啟動或使用者登入腳本，不應從命令列執行。

## <a name="additional-references"></a>其他參考

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [使用群組原則部署印表機](https://go.microsoft.com/fwlink/?LinkId=230627)