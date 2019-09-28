---
title: bitsadmin util 和 getieproxy
description: 適用于**bitsadmin util 和 getieproxy**的 Windows 命令主題-抓取指定服務帳戶的 proxy 使用方式。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6936e088ddcf467b5a8f931bc8217ba9da4662c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380267"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util 和 getieproxy

> 適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取指定服務帳戶的 proxy 使用方式。

## <a name="syntax"></a>語法

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|帳戶|指定您想要取得其 proxy 設定的服務帳戶。 可能值為：<br /><br />-LOCALSYSTEM<br />-NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|選擇性與 **/Conn**參數搭配使用，以指定要使用的數據機連接。 如果您未指定 **/Conn**參數，BITS 會使用 LAN 連接。 指定緊接在 **/Conn**參數後面的數據機連接名稱。|

## <a name="remarks"></a>備註

此參數會顯示每個 proxy 使用的值，而不只是您為服務帳戶指定的 proxy 使用方式。 如需設定服務帳戶之 proxy 使用方式的詳細資訊，請參閱[bitsadmin util 和 setieproxy](bitsadmin-util-and-setieproxy.md)參數。

## <a name="BKMK_examples"></a>典型

下列範例會顯示 NETWORK SERVICE 帳戶的 proxy 使用方式。

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>其他參考

[命令列語法關鍵](command-line-syntax-key.md)
