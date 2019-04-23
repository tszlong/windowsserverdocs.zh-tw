---
title: nslookup set recurse
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d1b7a93f-dfb0-4ccd-b230-e0953057fada
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ce2129a455ed5c8c2e4ef9540abb09102db2b5e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859339"
---
# <a name="nslookup-set-recurse"></a>nslookup set recurse



會要求網域名稱系統 (DNS) 名稱伺服器查詢其他伺服器，如果它沒有資訊。

## <a name="syntax"></a>語法

```
set [no]recurse
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|**norecurse**|停止查詢其他伺服器，如果它沒有資訊網域名稱系統 (DNS) 名稱伺服器。|
|**recurse**|會要求網域名稱系統 (DNS) 名稱伺服器查詢其他伺服器，如果它沒有資訊。 預設語法是**recurse**。|
|{說明 | ?}|顯示的簡短摘要**nslookup**子命令。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)