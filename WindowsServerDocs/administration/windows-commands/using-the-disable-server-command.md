---
title: 停用-伺服器
description: 停用伺服器的參考文章，會停用 Windows 部署服務 Server 的所有服務。
ms.topic: reference
ms.assetid: b69fcfe0-b744-4794-bc75-2c9218c0ba66
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8fbb6a08aef24b20a9bd1b0124b39624ae4ec919
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038176"
---
# <a name="disable-server"></a>停用-伺服器

停用 Windows 部署服務伺服器的所有服務。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Disable-Server [/Server:<Server name>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|

## <a name="examples"></a>範例

若要停用伺服器，請執行下列其中一項：
```
WDSUTIL /Disable-Server
WDSUTIL /Verbose /Disable-Server /Server:MyWDSServer
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

