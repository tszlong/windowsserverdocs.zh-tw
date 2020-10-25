---
title: wdsutil add-drivergrouppackages
description: Wdsutil add drivergrouppackages 命令的參考文章，此命令會新增驅動程式群組套件。
ms.topic: reference
ms.assetid: 29022f53-ce14-4b2d-a81a-679c18e022b2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 67e211207f2c715053d734d659d511c61337e393
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524533"
---
# <a name="wdsutil-add-drivergrouppackages"></a>wdsutil add-drivergrouppackages

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

新增驅動程式群組套件。

## <a name="syntax"></a>語法

```
wdsutil /add-DriverGroupPackages /DriverGroup:<Group Name> [/Server:<Server Name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /DriverGroup:`<Groupname>` | 指定新驅動程式群組的名稱。 |
| 伺服器`<Servername>` | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| Filtertype`<Filtertype>` | 指定要搜尋的驅動程式套件類型。 您可以在單一命令中指定多個屬性。 您也必須使用此選項指定 **/Operator** 和 **/Value** 。 有效值包括：<ul><li>PackageId</li><li>PackageName</li><li>PackageEnabled</li><li>Packagedateadded</li><li>PackageInfFilename</li><li>PackageClass<p>Install-packageprovider</li><li>PackageArchitecture</li><li>PackageLocale</li><li>PackageSigned</li><li>PackagedatePublished</li><li>Packageversion</li><li>Driverdescription</li><li>DriverManufacturer</li><li>DriverHardwareId</li><li>DrivercompatibleId</li><li>DriverExcludeId</li><li>DriverGroupId</li><li>DriverGroupName**</li></ul>. |
| 運算子`{Equal|NotEqual|GreaterOrEqual|LessOrEqual|Contains}` | 指定屬性與值之間的關聯性。 您只能以字串屬性指定 **Contains** 。 您只能使用 date 和 version 屬性指定 **相等**、 **NotEqual**、 **GreaterOrEqual** 和 **LessOrEqual** 。 |
| 值`<Value>` | 指定對應至 **/Filtertype**的用戶端值。 您可以為單一 **/Filtertype**指定多個值。 每個篩選器的可用值為：<ul><li>**PackageId** -指定有效的 GUID。 例如： {4d36e972-e325-11ce-bfc1-08002be10318}</li><li>**PackageName** -指定任何字串值</li><li>**PackageEnabled** -指定 **[是] 或 [** **否**]</li><li>**Packagedateadded** -以下列格式指定日期： YYYY/MM/DD</li><li>**PackageInfFilename** -指定任何字串值</li><li>**PackageClass** -指定有效的類別名稱或類別 GUID。 例如： **DiskDrive**、 **Net**或 {4d36e972-e325-11ce-bfc1-08002be10318}</li><li>**Install-packageprovider** -指定任何字串值</li><li>**PackageArchitecture** -指定 **x86**、 **x64**或 **ia64**</li><li>**PackageLocale** -指定有效的語言識別項。 例如： **en-us** 或 **es**</li><li>**PackageSigned** -指定 **[是] 或 [** **否**]</li><li>**PackagedatePublished** -以下列格式指定日期： YYYY/MM/DD</li><li>**Packageversion** -以下列格式指定版本： a.b.x.y。 例如：6.1.0。0</li><li>**Driverdescription** -指定任何字串值</li><li>**DriverManufacturer** -指定任何字串值</li><li>**DriverHardwareId** -指定任何字串值</li><li>**DrivercompatibleId** -指定任何字串值</li><li>**DriverExcludeId** -指定任何字串值</li><li>**DriverGroupId** -指定有效的 GUID。 例如： {4d36e972-e325-11ce-bfc1-08002be10318}</li><li>**DriverGroupName** -指定任何字串值</li></ul> 如需這些值的詳細資訊，請參閱 [驅動程式和封裝屬性](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759262(v=ws.11))。 |

## <a name="examples"></a>範例

若要新增驅動程式群組套件，請輸入下列其中一項：

```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:printerdrivers /Filtertype:PackageClass /Operator:Equal /Value:printer /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2
```

```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:DisplayDriversX86 /Filtertype:PackageClass /Operator:Equal /Value:Display /Filtertype:PackageArchitecture /Operator:Equal /Value:x86 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil add-driverpackage 命令](wdsutil-add-driverpackage.md)

- [wdsutil add-drivergrouppackage 命令](wdsutil-add-drivergrouppackage.md)

- [wdsutil add-alldriverpackages 命令](wdsutil-add-alldriverpackages.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
