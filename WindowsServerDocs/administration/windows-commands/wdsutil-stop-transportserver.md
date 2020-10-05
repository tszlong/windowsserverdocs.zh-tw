---
title: wdsutil 停止-transportserver
description: 適用于 stop-TransportServer 的參考文章
ms.topic: reference
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 800c99cba18ed2158749b1638627b31f8fdbaf5f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730519"
---
# <a name="wdsutil-stop-transportserver"></a>wdsutil 停止-transportserver

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停止傳輸伺服器上的所有服務。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定任何傳輸伺服器，將會使用本機伺服器。|
## <a name="examples"></a>範例
若要停止服務，請輸入下列其中一項：
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil disable-transportserver 命令](wdsutil-disable-transportserver.md)
- [wdsutil enable-transportserver 命令](wdsutil-enable-transportserver.md)
- [wdsutil get-transportserver 命令](wdsutil-get-transportserver.md)
- [wdsutil 設定-transportserver 命令](wdsutil-set-transportserver.md)
- [wdsutil 開始-transportserver 命令](wdsutil-start-transportserver.md)
