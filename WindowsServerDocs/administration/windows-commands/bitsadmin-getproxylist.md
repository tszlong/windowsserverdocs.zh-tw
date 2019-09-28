---
title: bitsadmin getproxylist-抓取指定之作業的 proxy 清單。
description: '**Bitsadmin getproxylist**的 Windows 命令主題-抓取指定之作業的 proxy 清單。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f176d268c816725b183da0a948afcb25272b2fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381309"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

抓取指定之作業的 proxy 清單。

## <a name="syntax"></a>語法

```
bitsadmin /GetProxyList <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

Proxy 清單是要使用的 proxy 伺服器清單。 清單是以逗號分隔。

## <a name="BKMK_examples"></a>典型

下列範例會抓取名為*myDownloadJob*之作業的 proxy 清單。
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)