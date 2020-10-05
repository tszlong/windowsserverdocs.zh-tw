---
title: wdsutil add-drivergrouppackages
description: 加入驅動程式群組套件的 wdsutil drivergrouppackages 的參考文章。
ms.topic: reference
ms.assetid: 29022f53-ce14-4b2d-a81a-679c18e022b2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6a6279c0cc8006f9f0b0673c1d3c3804b8cb3fb7
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730013"
---
# <a name="wdsutil-add-drivergrouppackages"></a>wdsutil add-drivergrouppackages

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

新增驅動程式群組套件。

## <a name="syntax"></a>語法
```
wdsutil /add-DriverGroupPackages /DriverGroup:<Group Name> [/Server:<Server Name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value>
```
### <a name="parameters"></a>參數

|                                         參數                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|--------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                 /DriverGroup:<Group Name>                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        指定驅動程式群組的名稱。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|                                   伺服器<Server name>                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|                                 Filtertype<Filter type>                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                指定要搜尋之驅動程式封裝的屬性。 您可以在單一命令中指定多個屬性。 您也必須使用此選項指定/Operator 和/Value。<p><Filter type> 可以是下列其中一項：<p>**PackageId**<p>**PackageName**<p>**PackageEnabled**<p>**Packagedateadded**<p>**PackageInfFilename**<p>**PackageClass**<p>**Install-packageprovider**<p>**PackageArchitecture**<p>**PackageLocale**<p>**PackageSigned**<p>**PackagedatePublished**<p>**Packageversion**<p>**Driverdescription**<p>**DriverManufacturer**<p>**DriverHardwareId**<p>**DrivercompatibleId**<p>**DriverExcludeId**<p>**DriverGroupId**<p>**DriverGroupName**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| /Operator： {等於 &#124; NotEqual &#124; GreaterOrEqual &#124; LessOrEqual &#124; Contains} |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   指定屬性與值之間的關聯性。 您只能以字串屬性指定 **Contains** 。 您只能使用 date 和 version 屬性指定 **相等**、 **NotEqual**、 **GreaterOrEqual** 和 **LessOrEqual** 。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                       值<Value>                                       | 指定對應至 **/Filtertype**的用戶端值。 您可以為單一 **/Filtertype**指定多個值。 下列清單概述您可以為每個篩選指定的值。 如需這些值的詳細資訊，請參閱 () 的 [驅動程式和封裝屬性](https://go.microsoft.com/fwlink/?LinkId=166895) <https://go.microsoft.com/fwlink/?LinkId=166895> 。<p>-PackageId-指定有效的 GUID。 例如： {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-PackageName 指定任何字串值。<br />-PackageEnabled-指定 **[是] 或 [** **否**]。<br />-Packagedateadded-以下列格式指定日期： YYYY/MM/DD<br />-PackageInfFilename 指定任何字串值。<br />-PackageClass-指定有效的類別名稱或類別 GUID。 例如： **DiskDrive**、 **Net**或 {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-Install-packageprovider 指定任何字串值。<br />-PackageArchitecture-指定 **x86**、 **x64**或 **ia64**。<br />-PackageLoale-指定有效的語言識別項。 例如： **en-us** 或 **es**。<br />-PackageSigned-指定 **[是] 或 [** **否**]。<br />-PackagedatePublished-以下列格式指定日期： YYYY/MM/DD<br />-Packageversion-以下列格式指定版本： a.b.x.y。 例如：6.1.0。0<br />-Driverdescription 指定任何字串值。<br />-DriverManufacturer 指定任何字串值。<br />-DriverHardwareId-指定任何字串值。<br />-DrivercompatibleId-指定任何字串值。<br />-DriverExcludeId-指定任何字串值。<br />-DriverGroupId-指定有效的 GUID。 例如： {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-DriverGroupName 指定任何字串值。 |

## <a name="examples"></a>範例
若要新增驅動程式套件，請輸入下列其中一項：
```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:printerdrivers /Filtertype:PackageClass /Operator:Equal /Value:printer /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2
```
```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:DisplayDriversX86 /Filtertype:PackageClass /Operator:Equal /Value:Display /Filtertype:PackageArchitecture /Operator:Equal /Value:x86 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-drivergrouppackage 命令](wdsutil-add-drivergrouppackage.md)
- [wdsutil add-driverpackage 命令](wdsutil-add-driverpackage.md)
- [wdsutil add-alldriverpackages 命令](wdsutil-add-alldriverpackages.md)