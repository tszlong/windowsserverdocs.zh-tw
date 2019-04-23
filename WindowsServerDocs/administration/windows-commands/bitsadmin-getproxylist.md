---
title: bitsadmin getproxylist-擷取指定作業的 proxy 清單。
description: 適用於 Windows 命令主題**bitsadmin getproxylist** -擷取指定作業的 proxy 清單。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e8c3ffb1e425552cda5b14a00287817ace77a90f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840509"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

擷取指定作業的 proxy 清單。

## <a name="syntax"></a>語法

```
bitsadmin /GetProxyList <Job>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|

## <a name="remarks"></a>備註

Proxy 清單是要使用的 proxy 伺服器的清單。 清單是以逗號分隔。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的 proxy 清單*myDownloadJob*。
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)