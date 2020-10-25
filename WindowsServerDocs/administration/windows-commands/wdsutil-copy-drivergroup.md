---
title: 複製-drivergroup
description: Drivergroup 命令的參考文章，此命令會在伺服器上複製現有的驅動程式群組，包括篩選器、驅動程式套件和啟用/停用狀態。
ms.topic: reference
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 924f6181e424dad88a33f1421098e658275d1fa3
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524913"
---
# <a name="copy-drivergroup"></a>複製-drivergroup

複製伺服器上現有的驅動程式群組，包括篩選器、驅動程式套件，以及啟用/停用狀態。

## <a name="syntax"></a>語法

```
wdsutil /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Groupname> /GroupName:<New Groupname>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| 伺服器`<Servername>` | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| /DriverGroup:`<Source Groupname>` | 指定來源驅動程式群組的名稱。 |
| GroupName`<New Groupname>` | 指定新驅動程式群組的名稱。 |

## <a name="examples"></a>範例

若要複製驅動程式群組，請輸入下列其中一項：

```
wdsutil /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```

```
wdsutil /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
