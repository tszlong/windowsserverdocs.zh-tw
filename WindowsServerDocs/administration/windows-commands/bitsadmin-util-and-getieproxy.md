---
title: bitsadmin util 和 getieproxy
description: 適用於 Windows 命令主題**bitsadmin util 和 getieproxy** -擷取指定的服務帳戶的 proxy 使用方式。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: de4f86340b1163c4d8e3286d9c86c9df794a21c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876869"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util 和 getieproxy

> 適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

擷取指定的服務帳戶的 proxy 使用方式。

## <a name="syntax"></a>語法

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|帳戶|指定您想要擷取其 proxy 設定服務帳戶。 可能值為：<br /><br />-LOCALSYSTEM<br />-   NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|搭配使用的選擇性 **/conn**參數來指定要使用的數據機連接。 如果您未指定 **/conn**參數，BITS 會使用區域網路連線。 指定的數據機連線名稱，緊接 **/conn**參數。|

## <a name="remarks"></a>備註

此參數會顯示每個 proxy 使用方式的值不只是 proxy 使用您指定的服務帳戶。 如需有關設定服務帳戶的 proxy 使用的詳細資訊，請參閱 < [bitsadmin util 和 setieproxy](bitsadmin-util-and-setieproxy.md)切換。

## <a name="BKMK_examples"></a>範例

下列範例會顯示在 NETWORK SERVICE 帳戶的 proxy 使用方式。

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
