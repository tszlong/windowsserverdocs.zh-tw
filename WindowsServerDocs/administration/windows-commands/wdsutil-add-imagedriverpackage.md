---
title: wdsutil add-imagedriverpackage
description: Wdsutil imagedriverpackage 的參考文章，會將驅動程式存放區中的驅動程式套件新增到伺服器上現有的開機映射。
ms.topic: reference
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5a85e64f39b360b5e970a15bd0da85326d67b06a
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730005"
---
# <a name="wdsutil-add-imagedriverpackage"></a>wdsutil add-imagedriverpackage

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將驅動程式存放區中的驅動程式套件新增至伺服器上現有的開機映射。 映射版本必須是 Windows 7 或 Windows Server 2008 R2 或更新版本。

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
|           /Server<Server name>           |                                                                                                                                               指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                |
|             媒體：<Image name>             |                                                                                                                                                                                       指定要新增驅動程式的映射名稱。                                                                                                                                                                                        |
|               媒體媒體：開機               |                                                                                                                                                                指定要新增驅動程式的映射類型。 驅動程式套件只能新增至開機映射。                                                                                                                                                                 |
| /Architecture： {x86 &#124; ia64 &#124; x64} |                                                                                                       指定開機映射的架構。 由於不同架構中的開機映射可以有相同的映射名稱，因此您應該指定架構，以確保使用正確的映射。                                                                                                        |
|           /Filename： <File name> ]           |                                                                                                                                                        指定檔案名。 如果映射無法依名稱唯一識別，則必須指定檔案名。                                                                                                                                                        |
|           [/DriverPackage:<Name>           |                                                                                                                                                                                   指定要新增至映射之驅動程式套件的名稱。                                                                                                                                                                                    |
|             [/PackageId： <ID> ]              | 指定驅動程式套件的 Windows 部署服務識別碼。 如果您無法依名稱唯一識別驅動程式套件，則必須指定此選項。 若要尋找封裝識別碼，請按一下套件所在的驅動程式群組 (或 [ **所有套件** ] 節點) 中，以滑鼠 **按右鍵封裝**，然後按一下 [內容]。 封裝識別碼會列在 [ **一般** ] 索引標籤上。例如： {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}。 |

## <a name="examples"></a>範例
若要將驅動程式套件新增至開機映射，請輸入下列其中一項：
```
wdsutil /add-ImageDriverPackagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-imagedriverpackages 命令](wdsutil-add-imagedriverpackages.md)
