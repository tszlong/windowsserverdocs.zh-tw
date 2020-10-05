---
title: wdsutil get-transportserver
description: Wdsutil transportserver 的參考文章，顯示指定傳輸伺服器的相關資訊。
ms.topic: reference
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a0d807035fa60ca757b1b27cc63c4bfce6e92070
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729888"
---
# <a name="wdsutil-get-transportserver"></a>wdsutil get-transportserver

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示指定傳輸伺服器的相關資訊。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Show： {Config}|傳回指定之傳輸伺服器的設定資訊。|
## <a name="examples"></a>範例
若要查看伺服器的相關資訊，請輸入：
```
wdsutil /Get-TransportServer /Show:Config
```
若要查看設定資訊，請輸入：
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil disable-transportserver 命令](wdsutil-disable-transportserver.md)
- [wdsutil enable-transportserver 命令](wdsutil-enable-transportserver.md)
- [wdsutil 設定-transportserver 命令](wdsutil-set-transportserver.md)
- [wdsutil 開始-transportserver 命令](wdsutil-start-transportserver.md)
- [wdsutil stop-transportserver 命令](wdsutil-stop-transportserver.md)
