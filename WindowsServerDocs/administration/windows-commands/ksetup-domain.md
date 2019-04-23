---
title: ksetup:domain
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1f53e807891b434709b1a8faed7aae8e8d444f6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857449"
---
# <a name="ksetupdomain"></a>ksetup:domain



設定所有的 Kerberos 作業的網域名稱。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DomainName>|您要建立連線的網域名稱。 使用完整的網域名稱或名稱，例如 contoso.com 或 contoso 的一個簡單的表單。|

## <a name="remarks"></a>備註

無。

## <a name="BKMK_Examples"></a>範例

連接到有效的網域，例如 Microsoft 使用 /mapuser 子命令：
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
連接成功時，您會收到新的 TGT 或現有的 TGT 將會重新整理。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)