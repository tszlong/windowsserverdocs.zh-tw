---
title: bitsadmin cancel
description: 適用於 Windows 命令主題**bitsadmin 取消**-從傳輸佇列中移除的作業，並刪除所有與作業相關聯的暫存檔案。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a4d1e2d6e4fd66cb525316f236d070fcd72d73f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814069"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

從傳輸佇列中移除的作業，並刪除所有與作業相關聯的暫存檔案。

## <a name="syntax"></a>語法

```
bitsadmin /cancel <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>範例

下列範例會移除*myDownloadJob*從傳輸佇列的作業。
```
C:\>bitsadmin /cancel myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)