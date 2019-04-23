---
title: 使用複製 DriverGroup 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 68d9c6f4ca78991bb4c286042a6172211161dd1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842079"
---
# <a name="using-the-copy-drivergroup-command"></a>使用複製 DriverGroup 命令



複製現有的驅動程式群組，包括篩選、 驅動程式套件，以及啟用/停用狀態的伺服器上。

## <a name="syntax"></a>語法

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[/ 伺服器：\<伺服器名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
|/ DriverGroup:\<來源群組名稱 >|指定來源驅動程式群組的名稱。|
|/ GroupName:\<新的群組名稱 >|指定新的驅動程式群組的名稱。|

## <a name="BKMK_examples"></a>範例

若要複製的驅動程式群組，請輸入下列其中一項：
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)