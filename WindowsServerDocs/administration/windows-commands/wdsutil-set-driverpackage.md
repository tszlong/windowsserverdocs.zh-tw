---
title: 子命令集-DriverPackage
description: 子命令集的參考文章-DriverPackage，它會重新命名及/或啟用或停用伺服器上的驅動程式套件。
ms.topic: reference
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a4b0b3154f6cc7cee34e6fdcc91f332295164541
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730554"
---
# <a name="subcommand-set-driverpackage"></a>子命令： set-DriverPackage

重新命名及/或啟用或停用伺服器上的驅動程式套件。

## <a name="syntax"></a>語法

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

### <a name="parameters"></a>參數

|        參數         |                                                                                                                                                                                                               描述                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： \<Server name> ] |                                                                                                                                                 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                 |
| [/DriverPackage： \<Name> ] |                                                                                                                                                                                       指定要修改之驅動程式套件的目前名稱。                                                                                                                                                                                        |
|    [/PackageId： \<ID> ]    | 指定驅動程式套件的 Windows 部署服務識別碼。 如果您無法依名稱唯一識別驅動程式套件，則必須指定此選項。 若要尋找封裝的此識別碼，請按一下套件所在的驅動程式群組 (或 [ **所有套件** ] 節點) 中，以滑鼠 **按右鍵封裝**，然後按一下 [內容]。 封裝識別碼會列在 [ **一般** ] 索引標籤上。例如： {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}。 |
|   [/Name： \<New Name> ]    |                                                                                                                                                                                              指定驅動程式套件的新名稱。                                                                                                                                                                                              |
|      [/Enabled： {Yes      |                                                                                                                                                                                                                   存在                                                                                                                                                                                                                    |

## <a name="examples"></a>範例

若要變更套件的設定，請輸入下列其中一項：
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)