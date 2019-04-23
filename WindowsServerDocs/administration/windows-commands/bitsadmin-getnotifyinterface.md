---
title: bitsadmin getnotifyinterface
description: 適用於 Windows 命令主題**bitsadmin getnotifyinterface** -判斷另一個程式是否已註冊 COM 回呼介面指定的工作。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8316721a20cc477f9e8e15fc57b5d1c861da3ff4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868039"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

判斷另一個程式是否已註冊 COM 回呼介面 （通知介面） 指定的工作。

## <a name="syntax"></a>語法

```
bitsadmin /GetNotifyInterface <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

顯示已註冊或已取消註冊。

> [!NOTE]
> 您不可以判斷已註冊的回呼介面的程式。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的通知介面*myDownloadJob*。
```
C:\>bitsadmin /GetNotifyInterface myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)