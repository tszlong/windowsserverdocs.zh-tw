---
title: bitsadmin addfile
description: '**Bitsadmin addfile**的 Windows 命令主題-將檔案新增至指定的作業。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8dfddda92e506dbfca2a47394a310edf16fe78aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382039"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

將檔案加入至指定的作業。

## <a name="syntax"></a>語法

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|RemoteURL|伺服器上檔案的 URL。|
|LocalName|本機電腦上的檔案名。 *LocalName*必須包含檔案的絕對路徑。|

## <a name="BKMK_examples"></a>典型

將檔案新增至作業。 針對您要新增的每個檔案重複此呼叫。 如果多個作業使用*myDownloadJob*作為其名稱，您必須將*myDownloadJob*取代為作業的 GUID，以唯一識別作業。
```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)