---
title: bitsadmin getproxyusage
description: '**Bitsadmin getproxyusage**的 Windows 命令主題-抓取指定工作的 proxy 使用方式設定。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea9a22f4fb35af3436d02d9f23b62ce0888a26b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381286"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage



抓取指定之作業的 proxy 使用方式設定。

## <a name="syntax"></a>語法

```
bitsadmin /GetProxyUsage <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

可能值為：
-   LNK-WHAT-ARE-PRECONFIG-SOLUTIONS —使用擁有者的 Internet Explorer 預設值。
-   NO_PROXY —請勿使用 PROXY 伺服器。
-   覆寫—使用明確的 proxy 清單。
-   自動偵測：自動偵測 proxy 設定。

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的 proxy 使用方式。
```
C:\>bitsadmin /GetProxyUsage myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)