---
title: wdsutil get-alldriverpackages
description: Wdsutil alldriverpackages 命令的參考文章，此命令會顯示伺服器上所有符合指定之搜尋準則的驅動程式套件的相關資訊。
ms.topic: reference
ms.assetid: 9eb8fcb7-ef46-4c8d-9623-8969a3c8b8a4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dd144d9abc2776f7e56913efd40775c1eef4d3ac
ms.sourcegitcommit: 28b5ab74cb0b40539ccc1a83998d6391e87fe51f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2020
ms.locfileid: "96614920"
---
# <a name="wdsutil-get-alldriverpackages"></a>wdsutil get-alldriverpackages

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示伺服器上所有符合指定之搜尋準則的驅動程式套件的相關資訊。

## <a name="syntax"></a>語法

```
wdsutil /get-alldriverpackages [/server:<servername>] [/show:{drivers | files | all}] [/filtertype:<filtertype> /operator:{equal | notequal | greaterorequal | lessorequal | contains} /value:<value> [/value:<value> ...]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `[/server:<servername>] `| 伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| `[/show:{drivers | files | all}]` | 指出要顯示的封裝資訊。 如果未指定 **/show** ，預設值為只傳回驅動程式封裝中繼資料。 **驅動程式** 會顯示套件中的驅動程式清單 **、檔案** 會顯示套件中的檔案清單，並顯示驅動 **程式和檔案** 。 |
| `/filtertype:<filtertype>` | 指定要搜尋之驅動程式封裝的屬性。 您可以在單一命令中指定多個屬性。 您也必須使用此選項指定 **/operator** 和 **/value** 。<p>`<filtertype>`可以是下列其中一項：<ul><li>PackageId</li><li>PackageName</li><li>PackageEnabled</li><li>Packagedateadded</li><li>PackageInfFilename</li><li>PackageClass</li><li>Install-packageprovider</li><li>PackageArchitecture</li><li>PackageLocale</li><li>PackageSigned</li><li>PackagedatePublished</li><li>Packageversion</li><li>Driverdescription</li><li>DriverManufacturer</li><li>DriverHardwareId</li><li>DrivercompatibleId</li><li>DriverGroupId</li><li>DriverGroupName</li></ul> |
| `/operator:{equal | notequal | greaterorequal | lessorequal | contains}` | 指定屬性與值之間的關聯性。 您只能以字串屬性指定 **contains** 。 您只能使用 date 和 version 屬性指定 **greaterorequal** 和 **lessorequal** 。 |
| `/value:<value>` | 指定要搜尋指定之的值 `<attribute>` 。 您可以為單一 **/filtertype** 指定多個值。 下列清單概述您可以針對每個篩選器指定的屬性。 如需這些屬性的詳細資訊，請參閱 [驅動程式和封裝屬性](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759262(v=ws.11))。 這些屬性可以包括：<ul><li>**PackageId.** 指定有效的 GUID。 例如： {4d36e972-e325-11ce-bfc1-08002be10318}。</li><li>**PackageName.** 指定任何字串值。</li><li>**PackageEnabled.** 指定 *[是] 或 [* *否*]。</li><li>**Packagedateadded.** 以下列格式指定日期： YYYY/MM/DD</li><li>**PackageInfFilename.** 指定任何字串值。</li><li>**PackageClass.** 指定有效的類別名稱或類別 GUID。 例如： *DiskDrive*、 *Net* 或 *{4d36e972-e325-11ce-bfc1-08002be10318}*。</li><li>**Install-packageprovider.** 指定任何字串值。</li><li>**PackageArchitecture.** 指定 *x86*、 *x64* 或 *ia64*。</li><li>**PackagLocale.** 指定有效的語言識別項。 例如： *en-us* 或 *es*。</li><li>**PackageSigned.** 指定 **[是] 或 [** **否**]。</li><li>**PackagedatePublished.** 以下列格式指定日期： YYYY/MM/DD。</li><li>**Packageversion.** 以下列格式指定版本： a.b.x.y。 例如：6.1.0.0。</li><li>**Driverdescription.** 指定任何字串值。</li><li>**DriverManufacturer.** 指定任何字串值。</li><li>**DriverHardwareId.** 指定任何字串值。</li><li>**DrivercompatibleId.** 指定任何字串值。</li><li>**DriverExcludeId.** 指定任何字串值。</li><li>**DriverGroupId.** 指定有效的 GUID。 例如： {4d36e972-e325-11ce-bfc1-08002be10318}。</li><li>**DriverGroupName.** 指定任何字串值。</li></ul> |

## <a name="examples"></a>範例

若要顯示資訊，請輸入下列其中一項：

```
wdsutil /get-alldriverpackages /server:MyWdsServer /show:all /filtertype:drivergroupname /operator:contains /value:printer /filtertype:packagearchitecture /operator:equal /value:x64 /value:x86
```

```
wdsutil /get-alldriverpackages /show:drivers /filtertype:packagedateadded /operator:greaterorequal /value:2008/01/01
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil get-driverpackage 命令](wdsutil-get-driverpackage.md)

- [wdsutil get-driverpackagefile 命令](wdsutil-get-driverpackagefile.md)
