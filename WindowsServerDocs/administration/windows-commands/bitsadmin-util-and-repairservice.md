---
title: bitsadmin util 和 repairservice
description: 適用於 Windows 命令主題**bitsadmin util 和 repairservice** -命令用來修正的 BITS 服務的各種版本的已知的問題。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bc5101378a389c865f5753146b711be0d15c6785
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852089"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util 和 repairservice

如果位元，就無法啟動，請使用此參數以修正各種位元版本的已知的問題。

**BITSAdmin 1.5 和更早版本：** 不受支援。

## <a name="syntax"></a>語法

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Force|選擇性： 刪除並重新建立服務。|

## <a name="remarks"></a>備註

此參數會解析錯誤有關： 不正確的服務組態和網路目錄 （例如 LANManworkstation) 的 Windows 服務的相依性。 此參數會產生輸出，指出問題，是否已解決。

> [!NOTE]
> 如果位元會重新建立服務，可能為英文設定服務描述字串，在當地語系化的系統中。

> [!IMPORTANT]
> 此命令不支援在 Windows Vista 上。

## <a name="BKMK_examples"></a>範例

下列範例會修復 BITS 服務組態。
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)