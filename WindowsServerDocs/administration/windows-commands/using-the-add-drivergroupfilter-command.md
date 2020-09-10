---
title: 新增-DriverGroupFilter
description: DriverGroupFilter 的參考文章，可將篩選加入至伺服器上的驅動程式群組。
ms.topic: reference
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bf11cbe86242a8051b173aa23f53748c20aa4ca6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626519"
---
# <a name="add-drivergroupfilter"></a>新增-DriverGroupFilter

將篩選新增至伺服器上的驅動程式群組。

## <a name="syntax"></a>語法

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

### <a name="parameters"></a>參數

|         參數          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:\<Group Name> |                                                                                                                                                                                                                                                                                                                                                                                                                                                              指定驅動程式群組的名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|  [/Server： \<Server name> ]  |                                                                                                                                                                                                                                                                                                                                                                                                               指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                                                                                                                                                                                                                                                                               |
| FilterType\<FilterType>  |                                                                                                                                                                                                   指定要加入至群組的篩選準則類型。 您可以在單一命令中指定多個篩選器類型。 每個篩選類型後面都必須接著 **/policy** ，並且至少包含一個 **/Value**。 \<FilterType> 可以是 **BiosVendor**、 **BiosVersion**、 **ChassisType**、 **Manufacturer**、 **Uuid**、 **OsVersion**、 **OsEdition**或 **OsLanguage**。 如需取得其他所有篩選器類型值的相關資訊，請參閱 () 的 [驅動程式群組篩選](https://go.microsoft.com/fwlink/?LinkID=155158) <https://go.microsoft.com/fwlink/?LinkID=155158> 。                                                                                                                                                                                                    |
|     [/Policy： {Include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             排除}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/Value： \<Value> ]      | 指定對應至 **/FilterType**的用戶端值。 您可以為單一類型指定多個值。 請參閱下列清單以取得 **ChassisType**的有效值。 如需取得其他所有篩選器類型值的相關資訊，請參閱 () 的 [驅動程式群組篩選](https://go.microsoft.com/fwlink/?LinkID=155158) <https://go.microsoft.com/fwlink/?LinkID=155158> 。</br>**其他**</br>**UnknownChassis**</br>**Desktop** (電腦)</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**塔**</br>**可擕式**</br>**[膝上型電腦]**</br>**Notebook**</br>**手持**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**午餐 盒**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

## <a name="examples"></a>範例

若要將篩選新增至驅動程式群組，請輸入下列其中一項：
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

