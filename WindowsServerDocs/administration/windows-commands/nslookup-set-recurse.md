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
ms.openlocfilehash: de948d9e182cf6489c1869a5725bce8319484293
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436669"
---
# <a name="nslookup-set-recurse"></a>nslookup set recurse



會要求網域名稱系統 (DNS) 名稱伺服器查詢其他伺服器，如果它沒有資訊。

## <a name="syntax"></a>語法

```
set [no]recurse
```

## <a name="parameters"></a>參數

|   參數   |                                                                  描述                                                                  |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| **norecurse** |                停止查詢其他伺服器，如果它沒有資訊網域名稱系統 (DNS) 名稱伺服器。                |
|  **recurse**  | 會要求網域名稱系統 (DNS) 名稱伺服器查詢其他伺服器，如果它沒有資訊。 預設語法是**recurse**。 |
|     {說明     |                                                                      ?}                                                                       |

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)