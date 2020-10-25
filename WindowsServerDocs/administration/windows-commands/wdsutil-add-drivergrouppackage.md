---
title: wdsutil add-drivergrouppackage
description: Wdsutil add drivergrouppackage 命令的參考文章，此命令會將驅動程式套件新增至驅動程式群組。
ms.topic: reference
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4bf7b7c6cae4551e09fa5aa7d0244e5e4f0407e4
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524543"
---
# <a name="wdsutil-add-drivergrouppackage"></a>wdsutil add-drivergrouppackage

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將驅動程式套件新增至驅動程式群組。

## <a name="syntax"></a>語法

```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /DriverGroup:`<Groupname>` | 指定新驅動程式群組的名稱。 |
| 伺服器`<Servername>` | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| /DriverPackage:`<Name>` | 指定要新增至群組之驅動程式套件的名稱。 如果您無法依名稱唯一識別驅動程式套件，則必須指定此選項。 |
| PackageId`<ID>` | 指定封裝的識別碼。 若要尋找封裝識別碼，請選取封裝所在的驅動程式群組 (或 [ **所有套件** ] 節點) ，以滑鼠右鍵按一下封裝，然後選取 [ **屬性**]。 封裝識別碼會列在 [ **一般** ] 索引標籤上，例如： **{DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}**。 |

## <a name="examples"></a>範例

若要新增驅動程式群組套件，請輸入下列其中一項：

```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```

```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil add-drivergroupfilter 命令](wdsutil-add-drivergroupfilter.md)

- [wdsutil add-drivergrouppackages 命令](wdsutil-add-drivergrouppackages.md)

- [wdsutil add-drivergroup 命令](wdsutil-add-drivergroup.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
