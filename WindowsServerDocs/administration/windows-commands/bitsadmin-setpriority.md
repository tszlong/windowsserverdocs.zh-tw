---
title: bitsadmin setpriority
description: 適用于 bitsadmin setpriority 的 Windows 命令主題，其會設定指定之作業的優先順序。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d007c62402a3d70910e1c79fab5c406295a63a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849211"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

設定指定之作業的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /SetPriority <Job> <Priority>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|優先順序|下列其中一個值：</br>-前景</br>-高</br>-正常</br>-低|

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會將名為*myDownloadJob*之作業的優先順序設定為 normal。
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)