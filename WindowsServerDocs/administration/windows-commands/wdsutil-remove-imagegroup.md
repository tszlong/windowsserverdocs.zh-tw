---
title: wdsutil 移除-imagegroup
description: Wdsutil imagegroup 的參考文章，它會從伺服器移除映射群組。
ms.topic: reference
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c9691281a706a2929c7406bfcca2c3f3410112e0
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730513"
---
# <a name="wdsutil-remove-imagegroup"></a>wdsutil 移除-imagegroup

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從伺服器移除映射群組。

## <a name="syntax"></a>語法
```
wdsutil [Options] /remove-ImageGroup Group:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|imagegroup:<Image group name>|指定要移除之映射群組的名稱|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要移除映射群組，請輸入下列其中一項：
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-imagegroup 命令](wdsutil-add-imagegroup.md)
- [wdsutil get-allimagegroups 命令](wdsutil-get-allimagegroups.md)
- [wdsutil get-imagegroup 命令](wdsutil-get-imagegroup.md)
- [wdsutil 設定-imagegroup 命令](wdsutil-set-imagegroup.md)
