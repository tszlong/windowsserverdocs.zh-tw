---
title: 停用-伺服器
description: 停用伺服器的參考主題，這會停用 Windows 部署服務伺服器的所有服務。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b69fcfe0-b744-4794-bc75-2c9218c0ba66
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5df5c7e2f18cdda2aeeea22c209881077c681f03
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720967"
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
|[/Server：\<伺服器名稱>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|

## <a name="examples"></a>範例

若要停用伺服器，請執行下列其中一項：
```
WDSUTIL /Disable-Server
WDSUTIL /Verbose /Disable-Server /Server:MyWDSServer
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

