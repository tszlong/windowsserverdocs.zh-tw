---
title: bitsadmin getnoprogresstimeout
description: '**Bitsadmin getnoprogresstimeout**的 Windows 命令主題-抓取服務在發生暫時性錯誤之後，嘗試傳輸檔案的時間長度（以秒為單位）。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7dcc0e445f4cae25c27f5ff70c73f4f2f23975aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381499"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout



抓取在暫時性錯誤發生後，服務嘗試傳輸檔案的時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /GetNoProgressTimeout <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的進度時間輸出值。
```
C:\>bitsadmin /GetNoProgressTimeout myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)