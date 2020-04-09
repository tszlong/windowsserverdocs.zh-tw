---
title: bitsadmin util 和 repairservice
description: 適用于 bitsadmin util 和 repairservice 的 Windows 命令主題，可修正各種 BITS 服務版本中的已知問題。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aaaa6edab22031dc53d266984bb669634e3bb362
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848891"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util 和 repairservice

如果 BITS 無法啟動，請使用此參數來修正各種版本的 BITS 中的已知問題。

**BITSAdmin 1.5 和更早版本：**  不受支援。

## <a name="syntax"></a>語法

```
bitsadmin /Util /RepairService [/Force]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Force|選擇性：刪除並重新建立服務。|

## <a name="remarks"></a>備註

此交換器會解決與 Windows 服務（例如 LANManworkstation）和網路目錄不正確的服務設定和相依性相關的錯誤。 此參數會產生輸出，指出是否已解決問題。

> [!NOTE]
> 如果 BITS 重新建立服務，則在當地語系化的系統中，服務描述字串可能會設定為英文。

> [!IMPORTANT]
> Windows Vista 不支援這個命令。

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會修復 BITS 服務設定。
```
C:\>bitsadmin /Util /RepairService
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)