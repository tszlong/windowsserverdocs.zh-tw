---
title: 使用 DriverGroupFilter 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a332c804fd3c78598eb9b1a6ba8cb5bdae0b3f1c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363826"
---
# <a name="using-the-add-drivergroupfilter-command"></a>使用 DriverGroupFilter 命令



將篩選新增至伺服器上的驅動程式群組。

## <a name="syntax"></a>語法

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

## <a name="parameters"></a>參數

|         參數          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup： \<Group 名稱 > |                                                                                                                                                                                                                                                                                                                                                                                                                                                              指定驅動程式群組的名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|  [/Server： \<Server 名稱 >]  |                                                                                                                                                                                                                                                                                                                                                                                                               指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                                                                                                                                                                                                                                                                               |
| /FilterType： \<FilterType >  |                                                                                                                                                                                                   指定要加入至群組的篩選準則類型。 您可以在單一命令中指定多個篩選準則類型。 每個篩選器類型後面必須加上 **/policy** ，而且至少要包含一個 **/Value**。 @no__t 0FilterType > 可以是**BiosVendor**、 **BiosVersion**、 **ChassisType**、 **Manufacturer**、 **Uuid**、 **OsVersion**、 **OsEdition**或**OsLanguage**。 如需取得所有其他篩選準則類型之值的詳細資訊，請參閱[驅動程式群組篩選器](https://go.microsoft.com/fwlink/?LinkID=155158)（<https://go.microsoft.com/fwlink/?LinkID=155158>）。                                                                                                                                                                                                    |
|     [/Policy： {Include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             排除}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/Value： \<Value >]      | 指定對應至 **/FilterType**的用戶端值。 您可以為單一類型指定多個值。 請參閱下列清單，以取得**ChassisType**的有效值。 如需取得所有其他篩選準則類型之值的詳細資訊，請參閱[驅動程式群組篩選器](https://go.microsoft.com/fwlink/?LinkID=155158)（<https://go.microsoft.com/fwlink/?LinkID=155158>）。</br>**另一方面**</br>**UnknownChassis**</br>**桌面**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**梯**</br>**台**</br>**膝上型電腦**</br>**筆記型電腦**</br>**掌上型**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

## <a name="BKMK_examples"></a>典型

若要將篩選新增至驅動程式群組，請輸入下列其中一項：
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

