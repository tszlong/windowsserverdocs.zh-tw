---
title: 新增-ImageDriverPackage
description: 適用于 ImageDriverPackage 的 Windows 命令主題，它會將驅動程式存放區中的驅動程式套件新增至伺服器上的現有開機映射。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b61a5beabed655ae629ad5c1e43d681568d07db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831921"
---
# <a name="add-imagedriverpackage"></a>新增-ImageDriverPackage

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將驅動程式存放區中的驅動程式套件新增至伺服器上的現有開機映射。 映射版本必須是 Windows 7 或 Windows Server 2008 R2 或更新版本。

## <a name="syntax"></a>語法
```
wdsutil /add-ImageDriverPackage [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
```
```
[/Filename:<File name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>參數

|                 參數                  |                                                                                                                                                                                                            描述                                                                                                                                                                                                             |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           [/Server：<Server name>           |                                                                                                                                               指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                |
|             媒體：<Image name>             |                                                                                                                                                                                       指定要新增驅動程式的映射名稱。                                                                                                                                                                                        |
|               媒體依據：開機               |                                                                                                                                                                指定要新增驅動程式的映射類型。 驅動程式套件只能新增至開機映射。                                                                                                                                                                 |
| /Architecture： {x86 &#124; ia64 &#124; x64} |                                                                                                       指定開機映射的架構。 由於不同架構中的開機映射可能會有相同的映射名稱，因此您應該指定架構，以確保使用正確的映射。                                                                                                        |
|           /Filename：<File name>]           |                                                                                                                                                        指定檔案名。 如果無法以名稱唯一識別映射，則必須指定檔案名。                                                                                                                                                        |
|           [/DriverPackage：<Name>           |                                                                                                                                                                                   指定要新增至映射的驅動程式套件名稱。                                                                                                                                                                                    |
|             [/PackageId：<ID>]              | 指定驅動程式套件的 Windows 部署服務識別碼。 如果驅動程式套件無法以名稱唯一識別，您就必須指定此選項。 若要尋找封裝識別碼，請按一下封裝所在的驅動程式群組（或 [**所有封裝**] 節點），以滑鼠右鍵按一下套件，然後按一下 [**屬性**]。 套件識別碼會列在 [**一般**] 索引標籤上。例如： {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型
若要將驅動程式套件新增至開機映射，請輸入下列其中一項：
```
wdsutil /add-ImageDriverPackagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
## <a name="additional-references"></a>其他參考資料
- [使用 ImageDriverPackages 命令的](using-the-add-imagedriverpackages-command.md)[命令列語法索引鍵](command-line-syntax-key.md)

