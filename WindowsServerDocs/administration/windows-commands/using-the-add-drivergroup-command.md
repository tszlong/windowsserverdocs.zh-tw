---
title: 使用新增 DriverGroup 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a92fe8f-03f9-462a-b99e-f23275259807
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 322a9a671f90bf56f6357289f7727c142a0145cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825089"
---
# <a name="using-the-add-drivergroup-command"></a>使用新增 DriverGroup 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將驅動程式群組新增到伺服器。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
## <a name="syntax"></a>語法
```
wdsutil /add-DriverGroup /DriverGroup:<Group Name>\n\
[/Server:<Server name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}] [/Filtertype:<Filter type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/DriverGroup:<Group Name>|指定新的驅動程式群組的名稱。|
|/ 伺服器：<Server name>|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
|/ 已啟用: {[是]&#124;無}|啟用或停用封裝。|
|/ 適用性: {比對&#124;所有}|指定要符合的篩選準則時安裝的套件。 **比對**表示安裝只符合用戶端的硬體的驅動程式套件。 **所有**表示安裝的所有套件至用戶端，不論硬體為何。|
|/ Filtertype:<Filtertype>|指定要加入至群組的篩選類型。 您可以在單一命令中指定多個篩選類型。 每個篩選類型後面必須接著 **/Policy**至少一個 **/值**。 <Filtertype> 可以是下列其中一項：<br /><br />**BiosVendor**<br /><br />**Biosversion**<br /><br />**Chassistype**<br /><br />**製造商**<br /><br />**uuid**<br /><br />**Osversion**<br /><br />**Osedition**<br /><br />**OsLanguage**<br /><br />取得值，所有其他篩選器類型的相關資訊，請參閱[驅動程式群組篩選器](https://go.microsoft.com/fwlink/?LinkID=155158)(https://go.microsoft.com/fwlink/?LinkID=155158)。|
|[/ 原則: {包含&#124;排除}]|指定要在篩選上設定的原則。 如果 **/Policy**設為**Include**，此群組中安裝的驅動程式允許用戶端電腦符合篩選條件。 如果 **/Policy**設為**排除**，則符合篩選條件的用戶端電腦不允許此群組中安裝的驅動程式。|
|[/ 值：<Value>]|指定對應至用戶端值 **/Filtertype**。 您可以指定單一類型的多個值。 請參閱下列清單以取得特定篩選器類型的有效值。 下列屬性適用於**Chassistype**。 取得所有其他篩選器類型值的相關資訊，請參閱[驅動程式群組篩選器](https://go.microsoft.com/fwlink/?LinkID=155158)(https://go.microsoft.com/fwlink/?LinkID=155158)。<br /><br />**其他**<br /><br />**UnknownChassis**<br /><br />**桌面**<br /><br />**LowProfileDesktop**<br /><br />**PizzaBox**<br /><br />**MiniTower**<br /><br />**塔**<br /><br />**可攜式**<br /><br />**Laptop**<br /><br />**筆記型電腦**<br /><br />**掌上型**<br /><br />**DockingStation**<br /><br />**AllInOne**<br /><br />**SubNotebook**<br /><br />**SpaceSaving**<br /><br />**LunchBox**<br /><br />**MainSystemChassis**<br /><br />**ExpansionChassis**<br /><br />**SubChassis**<br /><br />**BusExpansionChassis**<br /><br />**PeripheralChassis**<br /><br />**StoraeChassis**<br /><br />**RackmountChassis**<br /><br />**SealedCasecomputer**<br /><br />**MultiSystemChassis**<br /><br />**compactPci**<br /><br />**AdvancedTca**|
## <a name="BKMK_examples"></a>範例
若要新增驅動程式群組，請輸入下列其中一項：
```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Applicability:All /Filtertype:Manufacturer /Policy:Include /Value:Name1 /Filtertype:Chassistype /Policy:Exclude /Value:Tower /Value:MiniTower
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增 DriverGroupPackage 命令](using-the-add-drivergrouppackage-command.md)
[使用 新增 DriverGroupPackages 命令](using-the-add-drivergrouppackages-command.md)
 [使用新增 DriverGroupFilter 命令](using-the-add-drivergroupfilter-command.md)
