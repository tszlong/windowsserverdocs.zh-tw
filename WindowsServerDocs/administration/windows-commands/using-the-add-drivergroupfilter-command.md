---
title: 新增-DriverGroupFilter
description: 適用于 DriverGroupFilter 的 Windows 命令主題，它會將篩選新增至伺服器上的驅動程式群組。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a62baf85462d4340d61196bc154efd6d852f1f7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832111"
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
| /DriverGroup：\<組名 > |                                                                                                                                                                                                                                                                                                                                                                                                                                                              指定驅動程式群組的名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|  [/Server：\<伺服器名稱 >]  |                                                                                                                                                                                                                                                                                                                                                                                                               指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                                                                                                                                                                                                                                                                               |
| /FilterType：\<FilterType >  |                                                                                                                                                                                                   指定要加入至群組的篩選準則類型。 您可以在單一命令中指定多個篩選準則類型。 每個篩選器類型後面必須加上 **/policy** ，而且至少要包含一個 **/Value**。 \<FilterType > 可以是**BiosVendor**、 **BiosVersion**、 **ChassisType**、 **Manufacturer**、 **Uuid**、 **OsVersion**、 **OsEdition**或**OsLanguage**。 如需取得所有其他篩選準則類型之值的詳細資訊，請參閱[驅動程式群組篩選器](https://go.microsoft.com/fwlink/?LinkID=155158)（<https://go.microsoft.com/fwlink/?LinkID=155158>）。                                                                                                                                                                                                    |
|     [/Policy： {Include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             排除}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/Value：\<值 >]      | 指定對應至 **/FilterType**的用戶端值。 您可以為單一類型指定多個值。 請參閱下列清單，以取得**ChassisType**的有效值。 如需取得所有其他篩選準則類型之值的詳細資訊，請參閱[驅動程式群組篩選器](https://go.microsoft.com/fwlink/?LinkID=155158)（<https://go.microsoft.com/fwlink/?LinkID=155158>）。</br>**另一方面**</br>**UnknownChassis**</br>**桌面**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**梯**</br>**台**</br>**膝上型電腦**</br>**筆記型電腦**</br>**掌上型**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要將篩選新增至驅動程式群組，請輸入下列其中一項：
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

