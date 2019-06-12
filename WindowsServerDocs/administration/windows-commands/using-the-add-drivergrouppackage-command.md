---
title: 使用新增 DriverGroupPackage 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad9d180e2cf2110d36ebc82211af3a495a0e0b6b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440737"
---
# <a name="using-the-add-drivergrouppackage-command"></a>使用新增 DriverGroupPackage 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將驅動程式群組中的驅動程式套件。
## <a name="syntax"></a>語法
```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>參數

|         參數         |                                                                                                                                               描述                                                                                                                                               |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:<Group Name> |                                                                                                                                 指定驅動程式群組的名稱。                                                                                                                                 |
|   / 伺服器：<Server name>   |                                                                                  指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果沒有指定伺服器名稱時，會使用本機伺服器。                                                                                  |
|   /DriverPackage:<Name>   |                                                                      指定驅動程式套件新增至群組的名稱。 如果驅動程式套件不能唯一識別名稱，您必須指定此選項。                                                                       |
|      /PackageId:<ID>      | 指定封裝識別碼。 若要尋找封裝識別碼，請按一下 封裝位於驅動程式群組 (或**所有封裝**節點)，以滑鼠右鍵按一下封裝，然後按一下**屬性**。 套件識別碼會列在**一般**索引標籤，例如： **{DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}** 。 |

## <a name="BKMK_examples"></a>範例
若要新增驅動程式套件，請輸入下列其中一項：
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增 DriverGroupPackages 命令](using-the-add-drivergrouppackages-command.md)
[使用 新增 DriverPackage 命令](using-the-add-driverpackage-command.md)
 [使用新增 AllDriverPackages 子命令](using-the-add-alldriverpackages-subcommand.md)
