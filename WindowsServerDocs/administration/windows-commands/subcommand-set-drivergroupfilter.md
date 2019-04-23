---
title: 子命令集 DriverGroupFilter
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 339beda0d6e92c7632cb16566c8db7be5f1079af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835059"
---
# <a name="subcommand-set-drivergroupfilter"></a>Subcommand: set-DriverGroupFilter



從新增或移除現有的驅動程式群組篩選器驅動程式群組。

## <a name="syntax"></a>語法

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/ DriverGroup:\<群組名稱 >|指定驅動程式群組的名稱。|
|[/ 伺服器：\<伺服器名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/FilterType:\<FilterType>|指定要新增或移除的驅動程式群組篩選器類型。 您可以在單一命令中指定多個篩選條件。 每個 **/FilterType**，您可以新增或移除使用的多個值 **/RemoveValue**並 **/AddValue**。 \<FilterType > 可以是下列其中之一：</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**製造商**</br>**uuid**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|
|[/ 原則: {包含 | Exclude}]|指定要在篩選上設定新的原則。 如果 **/Policy**設為**Include**，此群組中安裝的驅動程式允許用戶端電腦符合篩選條件。 如果 **/Policy**設為**排除**，則符合篩選條件的用戶端電腦不允許此群組中安裝的驅動程式。|
|[/ AddValue:\<值 >]|指定新的用戶端值，以加入篩選器。 您可以指定單一篩選條件類型的多個值。 請參閱下列清單的有效屬性值**ChassisType**。 取得所有其他篩選器類型值的相關資訊，請參閱[驅動程式群組篩選器](https://go.microsoft.com/fwlink/?LinkID=155158)(https://go.microsoft.com/fwlink/?LinkID=155158)。</br>**其他**</br>**UnknownChassis**</br>**桌面**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**塔**</br>**可攜式**</br>**Laptop**</br>**筆記型電腦**</br>**掌上型**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca**|
|[/RemoveValue:\<Value>]|指定現有用戶端来移除的值為指定的篩選條件 **/AddValue**。|

## <a name="BKMK_examples"></a>範例

若要移除篩選，請輸入下列其中一項：
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)