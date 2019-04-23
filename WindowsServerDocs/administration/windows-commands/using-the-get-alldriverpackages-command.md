---
title: 使用 get AllDriverPackages 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9eb8fcb7-ef46-4c8d-9623-8969a3c8b8a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e531d9e175ba35aa092c1ef95ec5fe7d1eddff6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843649"
---
# <a name="using-the-get-alldriverpackages-command"></a>使用 get AllDriverPackages 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示符合指定的搜尋條件的伺服器上的所有驅動程式套件的相關資訊。
## <a name="syntax"></a>語法
```
wdsutil /Get-AllDriverPackages [/Server:<Server name>] [/Show:{Drivers | Files | All}] [/Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[] / [顯示: {驅動程式&#124;檔案&#124;所有}]|表示要顯示的套件資訊。 如果 **/ 顯示**未指定，預設值是要傳回的驅動程式套件中繼資料。 **驅動程式**顯示封裝中的驅動程式清單。 **檔案**顯示封裝中的檔案清單。 **所有**顯示驅動程式檔案和檔案。|
|/ Filtertype:<Filter type>|指定要搜尋的驅動程式套件的屬性。 您可以在單一命令中指定多個屬性。 您也必須指定 **/Operator**並 **/值**使用此選項。<br /><br /><Filter type> 可以是下列其中一項：<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/Operator:{Equal &#124; NotEqual &#124; GreaterOrEqual &#124; LessOrEqual &#124; Contains}|指定屬性和值之間的關聯性。 您可以指定**Contains**只能搭配字串屬性。 您可以指定**GreaterOrEqual**並**LessOrEqual**只使用日期和版本的屬性。|
|/ 值：<Value>|指定要搜尋指定的值<attribute>。  您可以指定多個值的單一 **/Filtertype**。 下表簡要描述您可以指定每個篩選器的屬性。 如需有關這些屬性的詳細資訊，請參閱 <<c0> [ 驅動程式和封裝屬性](https://go.microsoft.com/fwlink/?LinkId=166895)(https://go.microsoft.com/fwlink/?LinkId=166895)。<br /><br />-PackageId-指定有效的 GUID。 例如: {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-PackageName 指定任何字串值。<br />-PackageEnabled-指定 **[是]** 或是**No**。<br />-Packagedateadded-指定的日期，格式如下：YYYY/MM/DD<br />-PackageInfFilename 指定任何字串值。<br />-PackageClass-指定有效的類別名稱或類別 GUID。 例如: **DiskDrive**， **Net**，或 {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-PackageProvider 指定任何字串值。<br />-PackageArchitecture-指定**x86**， **x64**，或**ia64**。<br />-PackagLocale-指定有效的語言識別碼。 例如： **EN-US**或是**ES-ES**。<br />-PackageSigned-指定 **[是]** 或是**No**。<br />-PackagedatePublished-指定的日期，格式如下：YYYY/MM/DD<br />-Packageversion 指定的版本，格式如下： a.b.x.y. 例如: 6.1.0.0<br />-Driverdescription 指定任何字串值。<br />-DriverManufacturer 指定任何字串值。<br />-DriverHardwareId-指定任何字串值。<br />-DrivercompatibleId-指定任何字串值。<br />-DriverExcludeId 指定任何字串值。<br />-DriverGroupId 指定有效的 GUID。 例如: {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-DriverGroupName 指定任何字串值。|
## <a name="BKMK_examples"></a>範例
若要顯示的資訊，請輸入下列其中一項：
```
wdsutil /Get-AllDriverPackages /Server:MyWdsServer /Show:All /Filtertype:DriverGroupName /Operator:Contains /Value:printer /Filtertype:PackageArchitecture /Operator:Equal /Value:x64 /Value:x86
```
```
wdsutil /Get-AllDriverPackages /Show:Drivers /Filtertype:Packagedateadded /Operator:GreaterOrEqual /Value:2008/01/01
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 get DriverPackage 命令](using-the-get-driverpackage-command.md)
[使用 get DriverPackageFile 命令](using-the-get-driverpackagefile-command.md)
