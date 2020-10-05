---
title: wdsutil 設定-imagegroup
description: 子命令集的參考文章-ImageGroup，可變更映射群組的屬性。
ms.topic: reference
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6f3b2b3790ecc126f8be48ade61d305d44011f68
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730561"
---
# <a name="wdsutil-set-imagegroup"></a>wdsutil 設定-imagegroup

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更映射群組的屬性。

## <a name="syntax"></a>語法
```
wdsutil [Options] /set-imagegroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/set-imagegroup:<Image group name>|指定映射群組的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果沒有指定，則使用本機伺服器。|
|[/Name： <New image group name> ]|指定映射群組的新名稱。|
|[/Security： <SDDL> ]|以安全描述項定義語言 (SDDL) 格式，指定映射群組的新安全描述項。|
## <a name="examples"></a>範例
若要設定映射群組的名稱，請輸入：
```
wdsutil /Set-ImageGroup:ImageGroup1 /Name:New Image Group Name
```
若要指定映射群組的各種設定，請輸入：
```
wdsutil /verbose /Set-ImageGroupGroup:ImageGroup1 /Server:MyWDSServer /Name:New Image Group Name
/Security:O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-imagegroup 命令](wdsutil-add-imagegroup.md)
- [wdsutil get-allimagegroups 命令](wdsutil-get-allimagegroups.md)
- [wdsutil get-imagegroup 命令](wdsutil-get-imagegroup.md)
- [wdsutil remove-imagegroup 命令](wdsutil-remove-imagegroup.md)
