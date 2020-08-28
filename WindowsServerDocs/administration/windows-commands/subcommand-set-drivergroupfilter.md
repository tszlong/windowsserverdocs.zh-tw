---
title: 子命令集-DriverGroupFilter
description: 子命令集的參考文章-DriverGroupFilter，可從驅動程式群組新增或移除現有的驅動程式群組篩選器。
ms.topic: reference
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b480542a59f1c12858d0a133a778748411992827
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036866"
---
# <a name="subcommand-set-drivergroupfilter"></a>子命令： set-DriverGroupFilter

從驅動程式群組新增或移除現有的驅動程式群組篩選器。

## <a name="syntax"></a>語法

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

### <a name="parameters"></a>參數

|         參數          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:\<Group Name> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                 指定驅動程式群組的名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|  [/Server： \<Server name> ]  |                                                                                                                                                                                                                                                                                                                                                                                                                指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                                                                                                                                                                                                                                                                                 |
| FilterType\<FilterType>  |                                                                                                                                                                                                                                                                       指定要新增或移除的驅動程式群組篩選器類型。 您可以在單一命令中指定多個篩選準則。 針對每個 **/FilterType**，您可以使用 **/RemoveValue** 和 **/AddValue**來新增或移除多個值。 \<FilterType> 可以是下列其中一項：</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**製造商**</br>**Uuid**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**                                                                                                                                                                                                                                                                        |
|     [/Policy： {Include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                排除}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|    [/AddValue： \<Value> ]    | 指定要加入至篩選準則的新用戶端值。 您可以針對單一篩選類型指定多個值。 如需 **ChassisType**的有效屬性值，請參閱下列清單。 如需取得其他所有篩選器類型值的相關資訊，請參閱 () 的 [驅動程式群組篩選](https://go.microsoft.com/fwlink/?LinkID=155158) <https://go.microsoft.com/fwlink/?LinkID=155158> 。</br>**其他**</br>**UnknownChassis**</br>**桌上型電腦**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**塔**</br>**可擕式**</br>**[膝上型電腦]**</br>**Notebook**</br>**手持**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**午餐 盒**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |
|  [/RemoveValue： \<Value> ]   |                                                                                                                                                                                                                                                                                                                                                                                                                                     指定要從篩選器中移除的現有用戶端值（如 **/AddValue**所指定）。                                                                                                                                                                                                                                                                                                                                                                                                                                      |

## <a name="examples"></a>範例

若要移除篩選準則，請輸入下列其中一項：
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)