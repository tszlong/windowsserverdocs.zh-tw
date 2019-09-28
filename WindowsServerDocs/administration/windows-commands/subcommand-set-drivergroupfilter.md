---
title: 子命令集-DriverGroupFilter
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: efdfd3aa1725749d177f06ff23deb777529a95a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383854"
---
# <a name="subcommand-set-drivergroupfilter"></a>子命令： set-DriverGroupFilter



從驅動程式群組新增或移除現有的驅動程式群組篩選器。

## <a name="syntax"></a>語法

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

## <a name="parameters"></a>參數

|         參數          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup： \<Group 名稱 > |                                                                                                                                                                                                                                                                                                                                                                                                                                                                 指定驅動程式群組的名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|  [/Server： \<Server 名稱 >]  |                                                                                                                                                                                                                                                                                                                                                                                                                指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                                                                                                                                                                                                                                                                                 |
| /FilterType： \<FilterType >  |                                                                                                                                                                                                                                                                       指定要新增或移除的驅動程式群組篩選器類型。 您可以在單一命令中指定多個篩選準則。 針對每個 **/FilterType**，您可以使用 **/RemoveValue**和 **/AddValue**來新增或移除多個值。 @no__t 0FilterType > 可以是下列其中一項：</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**製造商**</br>**Uuid**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**                                                                                                                                                                                                                                                                        |
|     [/Policy： {Include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                排除}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|    [/AddValue： \<Value >]    | 指定要加入至篩選準則的新用戶端值。 您可以為單一篩選器類型指定多個值。 如需**ChassisType**的有效屬性值，請參閱下列清單。 如需取得所有其他篩選準則類型之值的詳細資訊，請參閱[驅動程式群組篩選器](https://go.microsoft.com/fwlink/?LinkID=155158)（<https://go.microsoft.com/fwlink/?LinkID=155158>）。</br>**另一方面**</br>**UnknownChassis**</br>**桌面**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**梯**</br>**台**</br>**膝上型電腦**</br>**筆記型電腦**</br>**掌上型**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |
|  [/RemoveValue： \<Value >]   |                                                                                                                                                                                                                                                                                                                                                                                                                                     指定要從篩選準則中移除的現有用戶端值（如 **/AddValue**所指定）。                                                                                                                                                                                                                                                                                                                                                                                                                                      |

## <a name="BKMK_examples"></a>典型

若要移除篩選，請輸入下列其中一項：
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)