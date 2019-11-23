---
title: bitsadmin util 和 repairservice
description: 適用于**bitsadmin util 和 repairservice**的 Windows 命令主題-用來修正各種 BITS 服務版本的已知問題。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ab06ac9c784cfa438eb285c28f0e661cf4b8302
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380285"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util 和 repairservice

如果 BITS 無法啟動，請使用此參數來修正各種版本 BITS 的已知問題。

**BITSAdmin 1.5 和更早版本：**  不受支援。

## <a name="syntax"></a>語法

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|Force|選擇性：刪除並重新建立服務。|

## <a name="remarks"></a>備註

此交換器會解決與 Windows 服務（例如 LANManworkstation）和網路目錄不正確的服務設定和相依性相關的錯誤。 此參數會產生輸出，指出是否已解決問題。

> [!NOTE]
> 如果 BITS 重新建立服務，則在當地語系化的系統中，服務描述字串可能會設定為英文。

> [!IMPORTANT]
> Windows Vista 不支援這個命令。

## <a name="BKMK_examples"></a>典型

下列範例會修復 BITS 服務設定。
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)