---
title: 使用新增 ImageDriverPackage 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 38d6c032f347f9945701f17b9289f3e3ff474031
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440618"
---
# <a name="using-the-add-imagedriverpackage-command"></a>使用新增 ImageDriverPackage 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

加入至現有的開機映像，在伺服器上的驅動程式存放區中的驅動程式套件。 映像版本必須是 Windows 7 或 Windows Server 2008 R2 或更新版本。
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
|           [/ 伺服器：<Server name>           |                                                                                                                                               指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果沒有指定伺服器名稱時，會使用本機伺服器。                                                                                                                                                |
|             媒體：<Image name>             |                                                                                                                                                                                       指定要新增驅動程式的映像的名稱。                                                                                                                                                                                        |
|               mediatype:Boot               |                                                                                                                                                                指定要新增驅動程式的影像類型。 驅動程式套件只能加入至開機映像。                                                                                                                                                                 |
| / 架構: {x86 &#124; ia64 &#124; x64} |                                                                                                       指定的開機映像的架構。 因為它可能會有將開機映像的相同映像名稱放在不同的架構，您應該指定以確保使用正確的映像的架構。                                                                                                        |
|           / Filename:<File name>]           |                                                                                                                                                        指定檔案的名稱。 如果映像無法依名稱來唯一識別，則必須指定檔案名稱。                                                                                                                                                        |
|           [/ DriverPackage:<Name>           |                                                                                                                                                                                   指定要新增到映像的驅動程式套件的名稱。                                                                                                                                                                                    |
|             [/PackageId:<ID>]              | 指定驅動程式套件的 Windows 部署服務識別碼。 如果驅動程式套件不能唯一識別名稱，您必須指定此選項。 若要尋找封裝識別碼，請按一下 封裝位於驅動程式群組 (或**所有封裝**節點)，以滑鼠右鍵按一下封裝，然後按一下**屬性**。 套件識別碼會列在**一般** 索引標籤。例如: {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}。 |

## <a name="BKMK_examples"></a>範例
若要將驅動程式套件新增至開機映像中，輸入下列其中一項：
```
wdsutil /add-ImageDriverPackagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增 ImageDriverPackages 命令](using-the-add-imagedriverpackages-command.md)
