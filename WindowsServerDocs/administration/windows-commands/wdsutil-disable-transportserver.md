---
title: wdsutil 停用-transportserver
description: Wdsutil transportserver 的參考文章，會停用傳輸伺服器的所有服務。
ms.topic: reference
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9d404c8f16e876a15e4b683dd3cceef60804fbdb
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729958"
---
# <a name="wdsutil-disable-transportserver"></a>wdsutil 停用-transportserver

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停用傳輸伺服器的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定要停用之傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定任何傳輸伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要停用伺服器，請輸入：
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil enable-transportserver 命令](wdsutil-enable-transportserver.md)
- [wdsutil get-transportserver 命令](wdsutil-get-transportserver.md)
- [wdsutil 設定-transportserver 命令](wdsutil-set-transportserver.md)
- [wdsutil 開始-transportserver 命令](wdsutil-start-transportserver.md)
- [wdsutil stop-transportserver 命令](wdsutil-stop-transportserver.md)
