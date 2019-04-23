---
title: 使用新增 DriverGroupPackages 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29022f53-ce14-4b2d-a81a-679c18e022b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad0f8c7ed202f0d6fccc11307b17b742c70c3e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842789"
---
# <a name="using-the-add-drivergrouppackages-command"></a>使用新增 DriverGroupPackages 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
## <a name="syntax"></a>語法
```
wdsutil /add-DriverGroupPackages /DriverGroup:<Group Name> [/Server:<Server Name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/DriverGroup:<Group Name>|指定驅動程式群組的名稱。|
|/ 伺服器：<Server name>|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
|/ Filtertype:<Filter type>|指定要搜尋的驅動程式套件的屬性。 您可以在單一命令中指定多個屬性。 您也必須指定 /Operator 和 /Value 使用此選項。<br /><br /><Filter type> 可以是下列其中一項：<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/Operator:{Equal &#124; NotEqual &#124; GreaterOrEqual &#124; LessOrEqual &#124; Contains}|指定屬性和值之間的關聯性。 您只能指定**Contains**的字串屬性。 您只能指定**相等**， **NotEqual**， **GreaterOrEqual**並**LessOrEqual**使用日期和版本的屬性。|
|/ 值：<Value>|指定對應的用戶端值 **/Filtertype**。 您可以指定多個值的單一 **/Filtertype**。 下列清單列出您可以指定每個篩選條件的值。 如需有關這些值的詳細資訊，請參閱 <<c0> [ 驅動程式和封裝屬性](https://go.microsoft.com/fwlink/?LinkId=166895)(https://go.microsoft.com/fwlink/?LinkId=166895)。<br /><br />-PackageId-指定有效的 GUID。 例如: {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-PackageName 指定任何字串值。<br />-PackageEnabled-指定 **[是]** 或是**No**。<br />-Packagedateadded-指定的日期，格式如下：YYYY/MM/DD<br />-PackageInfFilename 指定任何字串值。<br />-PackageClass-指定有效的類別名稱或類別 GUID。 例如: **DiskDrive**， **Net**，或 {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-PackageProvider 指定任何字串值。<br />-PackageArchitecture-指定**x86**， **x64**，或**ia64**。<br />-PackageLoale-指定有效的語言識別碼。 例如： **EN-US**或是**ES-ES**。<br />-PackageSigned-指定 **[是]** 或是**No**。<br />-PackagedatePublished-指定的日期，格式如下：YYYY/MM/DD<br />-Packageversion-以下列格式指定版本： a.b.x.y. 例如: 6.1.0.0<br />-Driverdescription 指定任何字串值。<br />-DriverManufacturer 指定任何字串值。<br />-DriverHardwareId-指定任何字串值。<br />-DrivercompatibleId-指定任何字串值。<br />-DriverExcludeId-指定任何字串值。<br />-DriverGroupId-指定有效的 GUID。 例如: {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-DriverGroupName 指定任何字串值。|
## <a name="BKMK_examples"></a>範例
若要新增驅動程式套件，請輸入下列其中一項：
```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:printerdrivers /Filtertype:PackageClass /Operator:Equal /Value:printer /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2
```
```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:DisplayDriversX86 /Filtertype:PackageClass /Operator:Equal /Value:Display /Filtertype:PackageArchitecture /Operator:Equal /Value:x86 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增 DriverGroupPackage 命令](using-the-add-drivergrouppackage-command.md)
[使用 新增 DriverPackage 命令](using-the-add-driverpackage-command.md)
 [使用新增 AllDriverPackages 子命令](using-the-add-alldriverpackages-subcommand.md)
