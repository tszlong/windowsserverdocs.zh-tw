---
title: ksetup：網域
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bfaa8a37ae4ee5c9669b09f27a73b3d016324dea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841581"
---
# <a name="ksetupdomain"></a>ksetup：網域



設定所有 Kerberos 作業的功能變數名稱。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /domain <DomainName>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DomainName >|您想要建立連接的網功能變數名稱稱。 使用完整功能變數名稱或簡單格式的名稱，例如 contoso.com 或 contoso。|

## <a name="remarks"></a>備註

None。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

使用/mapuser 子命令建立與有效網域的連線，例如 Microsoft：
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
連線成功時，您會收到新的 TGT，或將重新整理現有的 TGT。

## <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)