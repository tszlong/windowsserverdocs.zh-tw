---
title: 使用 ImageDriverPackage 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1069b7c63e8b3bbc28fd900e8c869afc8abc03bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363733"
---
# <a name="using-the-add-imagedriverpackage-command"></a>使用 ImageDriverPackage 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將驅動程式存放區中的驅動程式套件新增至伺服器上的現有開機映射。 映射版本必須是 Windows 7 或 Windows Server 2008 R2 或更新版本。
## <a name="syntax"></a>語法
```
wdsutil /add-ImageDriverPackage [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
```
```
[/Filename:<File name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>參數

|                 參數                  |                                                                                                                                                                                                            描述                                                                                                                                                                                                             |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           [/Server： <Server name>           |                                                                                                                                               指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                |
|             媒體： <Image name>             |                                                                                                                                                                                       指定要新增驅動程式的映射名稱。                                                                                                                                                                                        |
|               媒體依據：開機               |                                                                                                                                                                指定要新增驅動程式的映射類型。 驅動程式套件只能新增至開機映射。                                                                                                                                                                 |
| /Architecture： {x86 &#124; ia64 &#124; x64} |                                                                                                       指定開機映射的架構。 由於不同架構中的開機映射可能會有相同的映射名稱，因此您應該指定架構，以確保使用正確的映射。                                                                                                        |
|           /Filename： <File name>]           |                                                                                                                                                        指定檔案名。 如果無法以名稱唯一識別映射，則必須指定檔案名。                                                                                                                                                        |
|           [/DriverPackage： <Name>           |                                                                                                                                                                                   指定要新增至映射的驅動程式套件名稱。                                                                                                                                                                                    |
|             [/PackageId： <ID>]              | 指定驅動程式套件的 Windows 部署服務識別碼。 如果驅動程式套件無法以名稱唯一識別，您就必須指定此選項。 若要尋找封裝識別碼，請按一下封裝所在的驅動程式群組（或 [**所有封裝**] 節點），以滑鼠右鍵按一下套件，然後按一下 [**屬性**]。 套件識別碼會列在 [**一般**] 索引標籤上。例如： {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}。 |

## <a name="BKMK_examples"></a>典型
若要將驅動程式套件新增至開機映射，請輸入下列其中一項：
```
wdsutil /add-ImageDriverPackagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[使用 ImageDriverPackages 命令](using-the-add-imagedriverpackages-command.md)
