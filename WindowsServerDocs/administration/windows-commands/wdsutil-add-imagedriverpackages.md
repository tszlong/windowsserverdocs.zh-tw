---
title: wdsutil add-imagedriverpackages
description: Wdsutil add imagedriverpackages 命令的參考文章，此命令會將驅動程式套件從驅動程式存放區新增至開機映射。
ms.topic: reference
ms.assetid: 9dc78909-a4d1-42a2-af8f-21ebcbfe8302
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 74029ca7b2243603c832b74f59a9248353cea911
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524473"
---
# <a name="wdsutil-add-imagedriverpackages"></a>wdsutil add-imagedriverpackages

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從驅動程式存放區將驅動程式套件新增至開機映射。

## <a name="syntax"></a>語法

```
wdsutil /add-ImageDriverPackages [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| [/Server： `<Servername>` ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| [媒體： `<Imagename>` ] | 指定要新增驅動程式的映射名稱。 |
| [媒體：開機] | 指定要新增驅動程式的映射類型。 驅動程式套件只能新增至開機映射。 |
| [/Architecture： `{x86 | ia64 | x64}` ] | 指定開機映射的架構。 由於不同架構中的開機映射可以有相同的映射名稱，因此您應該指定架構，以確保使用正確的映射。 |
| [/Filename： `<Filename>` ] | 指定檔案名。 如果映射無法依名稱唯一識別，則必須指定檔案名。 |
| Filtertype`<Filtertype>` | 指定要搜尋之驅動程式封裝的屬性。 您可以在單一命令中指定多個屬性。 您也必須使用此選項指定 **/Operator** 和 **/Value** 。 有效值包括：<ul><li>PackageId</li><li>PackageName</li><li>PackageEnabled</li><li>Packagedateadded</li><li>PackageInfFilename</li><li>PackageClass<p>Install-packageprovider</li><li>PackageArchitecture</li><li>PackageLocale</li><li>PackageSigned</li><li>PackagedatePublished</li><li>Packageversion</li><li>Driverdescription</li><li>DriverManufacturer</li><li>DriverHardwareId</li><li>DrivercompatibleId</li><li>DriverExcludeId</li><li>DriverGroupId</li><li>DriverGroupName**</li></ul>. |
| 運算子`{Equal|NotEqual|GreaterOrEqual|LessOrEqual|Contains}` | 指定屬性與值之間的關聯性。 您只能以字串屬性指定 **Contains** 。 您只能使用 date 和 version 屬性指定 **GreaterOrEqual** 和 **LessOrEqual** 。 |
| 值`<Value>` | 指定要搜尋的相對於指定的值 `<attribute>` 。 您可以為單一 **/Filtertype**指定多個值。 每個篩選器的可用值為：<ul><li>**PackageId** -指定有效的 GUID。 例如： {4d36e972-e325-11ce-bfc1-08002be10318}</li><li>**PackageName** -指定任何字串值</li><li>**PackageEnabled** -指定 **[是] 或 [** **否**]</li><li>**Packagedateadded** -以下列格式指定日期： YYYY/MM/DD</li><li>**PackageInfFilename** -指定任何字串值</li><li>**PackageClass** -指定有效的類別名稱或類別 GUID。 例如： **DiskDrive**、 **Net**或 {4d36e972-e325-11ce-bfc1-08002be10318}</li><li>**Install-packageprovider** -指定任何字串值</li><li>**PackageArchitecture** -指定 **x86**、 **x64**或 **ia64**</li><li>**PackageLocale** -指定有效的語言識別項。 例如： **en-us** 或 **es**</li><li>**PackageSigned** -指定 **[是] 或 [** **否**]</li><li>**PackagedatePublished** -以下列格式指定日期： YYYY/MM/DD</li><li>**Packageversion** -以下列格式指定版本： a.b.x.y。 例如：6.1.0。0</li><li>**Driverdescription** -指定任何字串值</li><li>**DriverManufacturer** -指定任何字串值</li><li>**DriverHardwareId** -指定任何字串值</li><li>**DrivercompatibleId** -指定任何字串值</li><li>**DriverExcludeId** -指定任何字串值</li><li>**DriverGroupId** -指定有效的 GUID。 例如： {4d36e972-e325-11ce-bfc1-08002be10318}</li><li>**DriverGroupName** -指定任何字串值</li></ul> 如需這些值的詳細資訊，請參閱 [驅動程式和封裝屬性](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759262(v=ws.11))。 |

## <a name="examples"></a>範例

若要將驅動程式套件新增至開機映射，請輸入下列其中一項：

```
wdsutil /add-ImageDriverPackagemedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /Filtertype:DriverGroupName /Operator:Equal /Value:x86Bus /Filtertype:PackageProvider /Operator:Contains /Value:Provider1 /Filtertype:Packageversion /Operator:GreaterOrEqual /Value:6.1.0.0
```

```
wdsutil /verbose /add-ImageDriverPackagemedia: WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```

```
wdsutil /verbose /add-ImageDriverPackagemedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Value:System /Value:DiskDrive /Value:HDC /Value:SCSIAdapter
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
-
- [wdsutil add-imagedriverpackage 命令](wdsutil-add-imagedriverpackage.md)
-
- [wdsutil add-alldriverpackages 命令](wdsutil-add-alldriverpackages.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
