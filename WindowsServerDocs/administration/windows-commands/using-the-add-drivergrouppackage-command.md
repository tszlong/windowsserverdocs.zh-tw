---
title: 新增-DriverGroupPackage
description: DriverGroupPackage 的參考主題，它會將驅動程式套件新增至驅動程式群組。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4baf4f16740e65c432cc09ca24270ab479346ac2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721106"
---
# <a name="add-drivergrouppackage"></a>新增-DriverGroupPackage

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將驅動程式套件新增至驅動程式群組。

## <a name="syntax"></a>語法
```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>參數

|         參數         |                                                                                                                                               描述                                                                                                                                               |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:<Group Name> |                                                                                                                                 指定驅動程式群組的名稱。                                                                                                                                 |
|   伺服器<Server name>   |                                                                                  指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                  |
|   /DriverPackage:<Name>   |                                                                      指定要加入至群組之驅動程式套件的名稱。 如果驅動程式套件無法以名稱唯一識別，您就必須指定此選項。                                                                       |
|      PackageId<ID>      | 指定封裝的識別碼。 若要尋找封裝識別碼，請按一下封裝所在的驅動程式群組（或 [**所有封裝**] 節點），以滑鼠右鍵按一下套件，然後按一下 [**屬性**]。 套件識別碼會列在 [**一般**] 索引標籤上，例如： **{DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}**。 |

## <a name="examples"></a>範例
若要新增驅動程式套件，請輸入下列其中一項：
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
## <a name="additional-references"></a>其他參考
- [Command-Line Syntax Key](command-line-syntax-key.md) 
[使用 DriverGroupPackages 命令](using-the-add-drivergrouppackages-command.md)
的命令列語法索引鍵使用[AllDriverPackages 子](using-the-add-alldriverpackages-subcommand.md)命令的[DriverPackage 命令](using-the-add-driverpackage-command.md)

