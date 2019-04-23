---
title: bitsadmin getproxyusage
description: 適用於 Windows 命令主題**bitsadmin getproxyusage** -擷取指定作業的 proxy 使用方式設定。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 20ba418b8dfcf3d96d9b20b22e53797a232a13f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863879"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage



擷取指定作業的 proxy 使用方式設定。

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
-   預先設定，使用擁有者的 Internet Explorer 的預設值。
-   NO_PROXY — 不使用 proxy 伺服器。
-   覆寫，使用明確的 proxy 清單。
-   自動偵測，自動偵測 proxy 設定。

## <a name="BKMK_examples"></a>範例

下列範例會擷取名為作業的 proxy 使用方式*myDownloadJob*。
```
C:\>bitsadmin /GetProxyUsage myDownloadJob
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)