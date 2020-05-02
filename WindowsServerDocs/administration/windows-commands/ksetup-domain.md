---
title: ksetup：網域
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f127eaf33e9ef6d597851c31a4167ceaa3516abb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724679"
---
# <a name="ksetupdomain"></a>ksetup：網域



設定所有 Kerberos 作業的功能變數名稱。

## <a name="syntax"></a>語法

```
ksetup /domain <DomainName>
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DomainName>|您想要建立連接的網功能變數名稱稱。 使用完整功能變數名稱或簡單格式的名稱，例如 contoso.com 或 contoso。|

## <a name="remarks"></a>備註

無。

## <a name="examples"></a>範例

使用/mapuser 子命令建立與有效網域的連線，例如 Microsoft：
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
連線成功時，您會收到新的 TGT，或將重新整理現有的 TGT。

## <a name="additional-references"></a>其他參考

-   [Ksetup](ksetup.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)