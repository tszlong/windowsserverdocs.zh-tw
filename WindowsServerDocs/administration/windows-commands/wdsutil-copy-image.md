---
title: wdsutil 複製-影像
description: Wdsutil 複製映射的參考文章，會複製相同映射群組內的影像。
ms.topic: reference
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a04848d0b673697809af08b91ca91856d6491773
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729973"
---
# <a name="wdsutil-copy-image"></a>wdsutil 複製-影像

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

複製相同映射群組內的影像。 若要在映射群組之間複製映射，請使用 [wdsutilExport-image 命令](wdsutil-export-image.md) 命令，然後使用 [wdsutiladd-image](wdsutil-add-image.md) 命令命令。

## <a name="syntax"></a>語法
```
wdsutil [Options] /copy-Image image:<Image name> [/Server:<Server name>]
    imagetype:Install
     imageGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Name:<Name>
         /Filename:<File name>
         [/Description:<Description>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
| 圖像：<Image name>|指定要複製之映射的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
| imagetype：安裝|指定要複製的影像類型。 此選項必須設定為 [ **安裝**]。|
|\imageGroup： <Image group name> ]|指定包含要複製之映射的映射群組。 如果未指定映射群組，且伺服器上只有一個群組，則預設會使用該映射群組。 如果伺服器上有一個以上的映射群組，您必須指定映射群組。|
|[/Filename： <Filename> ]|指定要複製之映射的檔案名。 如果來源映射無法依名稱唯一識別，您必須指定檔案名。|
|/DestinationImage|指定目的地映射的設定，如下表所述。<p>-/Name： <Name> -設定要複製之影像的顯示名稱。<br />-/Filename： <Filename> -設定將包含影像複製之目的地影像檔案的名稱。<br />-[/Description： <Description>]-設定影像複製的描述。|
## <a name="examples"></a>範例
若要建立指定映射的複本，並將它命名為 WindowsVista，請輸入：
```
wdsutil /copy-Image image:Windows Vista with Office imagetype:Install /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim
```
若要建立指定映射的複本，請套用指定的設定，並將複製 WindowsVista 命名為 .wim，輸入：
```
wdsutil /verbose /Progress /copy-Image image:Windows Vista with Office /Server:MyWDSServe imagetype:Install imageGroup:ImageGroup1
/Filename:install.wim /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim /Description:This is a copy of the original Windows image with Office installed
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-image 命令](wdsutil-add-image.md)
- [wdsutil 匯出-image 命令](wdsutil-export-image.md)
- [wdsutil 取得映射命令](wdsutil-get-image.md)
- [wdsutil remove-image 命令](wdsutil-remove-image.md)
- [wdsutil 取代-image 命令](wdsutil-replace-image.md)
- [wdsutil 設定-image 命令](wdsutil-set-image.md)
