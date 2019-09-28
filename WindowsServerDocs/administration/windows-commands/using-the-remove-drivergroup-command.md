---
title: 使用 DriverGroup 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d22ae4e191c2110a0b8d4cc50c24c2f3ec4a7e60
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362926"
---
# <a name="using-the-remove-drivergroup-command"></a>使用 DriverGroup 命令



從伺服器移除驅動程式群組。

## <a name="syntax"></a>語法

```
WDSUTIL /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/DriverGroup： \<Group 名稱 >|指定要移除之驅動程式群組的名稱。|
|[/Server： \<Server 名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|

## <a name="BKMK_examples"></a>典型

若要移除驅動程式群組，請輸入下列其中一項：
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)