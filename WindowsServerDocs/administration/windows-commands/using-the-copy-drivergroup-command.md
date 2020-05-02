---
title: 複製 DriverGroup
description: 複製 DriverGroup 的參考主題，這會複製伺服器上現有的驅動程式群組，包括篩選器、驅動程式套件，以及啟用/停用狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc157e9ef6d07a45efe2a19221fb3a046b2f65c1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721006"
---
# <a name="copy-drivergroup"></a>複製 DriverGroup

複製伺服器上現有的驅動程式群組，包括篩選器、驅動程式套件，以及啟用/停用狀態。

## <a name="syntax"></a>語法

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[/Server：\<伺服器名稱>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/DriverGroup：\<來源組名>|指定來源驅動程式群組的名稱。|
|/GroupName：\<新組名>|指定新驅動程式群組的名稱。|

## <a name="examples"></a>範例

若要複製驅動程式群組，請輸入下列其中一項：
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)