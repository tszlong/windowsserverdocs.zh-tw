---
title: wdsutil 停用-伺服器
description: Wdsutil disable-server 命令的參考文章，此命令會停用 Windows 部署服務 server 的所有服務。
ms.topic: reference
ms.assetid: b69fcfe0-b744-4794-bc75-2c9218c0ba66
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d8af0a0c929e2bff341b2b5bff71d0838b09d418
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070390"
---
# <a name="wdsutil-disable-server"></a>wdsutil 停用-伺服器

停用 Windows 部署服務伺服器的所有服務。

## <a name="syntax"></a>語法

```
wdsutil [Options] /Disable-Server [/Server:<Server name>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| [/Server： `<Servername>` ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |

## <a name="examples"></a>範例

若要停用伺服器，請輸入下列其中一項：

```
wdsutil /Disable-Server
```

```
wdsutil /Verbose /Disable-Server /Server:MyWDSServer
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
