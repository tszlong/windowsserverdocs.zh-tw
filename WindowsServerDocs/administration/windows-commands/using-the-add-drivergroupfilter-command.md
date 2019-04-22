---
title: 使用新增 DriverGroupFilter 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 16962bd87fabd1ee689b962547c2b1aa470283c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817379"
---
# <a name="using-the-add-drivergroupfilter-command"></a>使用新增 DriverGroupFilter 命令



新增篩選器驅動程式群組的伺服器上。

## <a name="syntax"></a>語法

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/ DriverGroup:\<群組名稱 >|指定驅動程式群組的名稱。|
|[/ 伺服器：\<伺服器名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
|/FilterType:\<FilterType>|指定要新增至群組的篩選類型。 您可以在單一命令中指定多個篩選類型。 每個篩選類型後面必須接著 **/Policy** ，而且包含至少一個 **/值**。 \<FilterType > 可以是**BiosVendor**， **BiosVersion**， **ChassisType**，**製造商**， **Uuid**， **OsVersion**， **OsEdition**，或**OsLanguage**。 取得所有其他篩選器類型值的相關資訊，請參閱[驅動程式群組篩選器](https://go.microsoft.com/fwlink/?LinkID=155158)(https://go.microsoft.com/fwlink/?LinkID=155158)。|
|[/ 原則: {包含 | Exclude}]|如果 **/Policy**設為**Include**，此群組中安裝的驅動程式允許用戶端電腦符合篩選條件。 如果 **/Policy**設為**排除**，符合篩選條件的用戶端電腦不允許此群組中安裝的驅動程式。|
|[/ 值：\<值 >]|指定對應至用戶端值 **/FilterType**。 您可以指定單一類型的多個值。 請參閱下列清單的有效值**ChassisType**。 取得所有其他篩選器類型值的相關資訊，請參閱[驅動程式群組篩選器](https://go.microsoft.com/fwlink/?LinkID=155158)(https://go.microsoft.com/fwlink/?LinkID=155158)。</br>**其他**</br>**UnknownChassis**</br>**桌面**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**塔**</br>**可攜式**</br>**Laptop**</br>**筆記型電腦**</br>**掌上型**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca**|

## <a name="BKMK_examples"></a>範例

若要將篩選新增至驅動程式群組中，輸入下列其中一項：
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

