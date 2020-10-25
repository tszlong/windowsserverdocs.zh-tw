---
title: 停用-伺服器
description: 停用伺服器的參考文章，會停用 Windows 部署服務 Server 的所有服務。
ms.topic: reference
ms.assetid: b69fcfe0-b744-4794-bc75-2c9218c0ba66
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6ff13092b5d99cd007d30eee80076f18814e138d
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524163"
---
# <a name="disable-server"></a>停用-伺服器

停用 Windows 部署服務伺服器的所有服務。

## <a name="syntax"></a>語法

```
wdsutil [Options] /Disable-Server [/Server:<Server name>]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|

## <a name="examples"></a>範例

若要停用伺服器，請執行下列其中一項：
```
wdsutil /Disable-Server
wdsutil /Verbose /Disable-Server /Server:MyWDSServer
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

