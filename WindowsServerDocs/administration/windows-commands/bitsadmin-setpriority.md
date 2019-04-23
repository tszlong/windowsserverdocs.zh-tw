---
title: bitsadmin setpriority
description: 適用於 Windows 命令主題**bitsadmin setpriority** -指定工作的優先權設定。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 072f22ae8c928d427104062b8cbf0f8f42ac4416
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882209"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority



設定指定工作的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /SetPriority <Job> <Priority>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|Priority|下列其中一個值：</br>-前景</br>-高</br>-標準</br>-低|

## <a name="BKMK_examples"></a>範例

下列範例會設定名為作業的優先權*myDownloadJob*正常狀態。
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)