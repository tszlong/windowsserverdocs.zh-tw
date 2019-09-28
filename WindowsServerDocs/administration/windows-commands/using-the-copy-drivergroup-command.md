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
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c08ce616c9b0e2bf79c7f13f922e27d7f7f7ca62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363589"
---
# <a name="using-the-copy-drivergroup-command"></a>使用 DriverGroup 命令



複製伺服器上現有的驅動程式群組，包括篩選器、驅動程式套件，以及啟用/停用狀態。

## <a name="syntax"></a>語法

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[/Server： \<Server 名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/DriverGroup： \<Source 組名 >|指定來源驅動程式群組的名稱。|
|/GroupName： \<New 組名 >|指定新驅動程式群組的名稱。|

## <a name="BKMK_examples"></a>典型

若要複製驅動程式群組，請輸入下列其中一項：
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)