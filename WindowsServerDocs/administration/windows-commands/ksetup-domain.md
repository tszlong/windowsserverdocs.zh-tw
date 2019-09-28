---
title: ksetup：網域
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a4d9f09def32c7518046c25887f4154020c5d7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375122"
---
# <a name="ksetupdomain"></a>ksetup：網域



設定所有 Kerberos 作業的功能變數名稱。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DomainName >|您想要建立連接的網功能變數名稱稱。 使用完整功能變數名稱或簡單格式的名稱，例如 contoso.com 或 contoso。|

## <a name="remarks"></a>備註

無。

## <a name="BKMK_Examples"></a>典型

使用/mapuser 子命令建立與有效網域的連線，例如 Microsoft：
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
連線成功時，您會收到新的 TGT，或將重新整理現有的 TGT。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)