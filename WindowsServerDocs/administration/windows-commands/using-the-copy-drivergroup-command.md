---
title: 複製 DriverGroup
description: 複製 DriverGroup 的參考文章，它會複製伺服器上現有的驅動程式群組，包括篩選器、驅動程式套件，以及啟用/停用狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d9be90f065cc76e16b7b45135c60b5206c7bf581
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934084"
---
# <a name="copy-drivergroup"></a>複製 DriverGroup

複製伺服器上現有的驅動程式群組，包括篩選器、驅動程式套件，以及啟用/停用狀態。

## <a name="syntax"></a>語法

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/DriverGroup:\<Source Group Name>|指定來源驅動程式群組的名稱。|
|GroupName\<New Group Name>|指定新驅動程式群組的名稱。|

## <a name="examples"></a>範例

若要複製驅動程式群組，請輸入下列其中一項：
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)