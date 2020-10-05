---
title: wdsutil add-drivergrouppackage
description: Wdsutil drivergrouppackage 的參考文章，會將驅動程式套件新增至驅動程式群組。
ms.topic: reference
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9389bbf7421e8c41c32760b8a20d85c4e4b675ea
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730014"
---
# <a name="wdsutil-add-drivergrouppackage"></a>wdsutil add-drivergrouppackage

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
|   /DriverPackage:<Name>   |                                                                      指定要新增至群組之驅動程式套件的名稱。 如果您無法依名稱唯一識別驅動程式套件，則必須指定此選項。                                                                       |
|      PackageId<ID>      | 指定封裝的識別碼。 若要尋找封裝識別碼，請按一下套件所在的驅動程式群組 (或 [ **所有套件** ] 節點) 中，以滑鼠 **按右鍵封裝**，然後按一下 [內容]。 封裝識別碼會列在 [ **一般** ] 索引標籤上，例如： **{DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}**。 |

## <a name="examples"></a>範例
若要新增驅動程式套件，請輸入下列其中一項：
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-drivergrouppackages 命令](wdsutil-add-drivergrouppackages.md)
- [wdsutil add-driverpackage 命令](wdsutil-add-driverpackage.md)
- [wdsutil add-alldriverpackages 命令](wdsutil-add-alldriverpackages.md)
