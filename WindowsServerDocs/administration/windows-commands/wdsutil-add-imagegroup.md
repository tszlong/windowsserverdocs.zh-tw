---
title: wdsutil add-imagegroup
description: Wdsutil add-imagegroup 的參考文章，可將映射群組新增至 Windows 部署服務伺服器。
ms.topic: reference
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 97f1439a4212a770dff6f6c42837c531f08c3d25
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729994"
---
# <a name="wdsutil-add-imagegroup"></a>wdsutil add-imagegroup

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將映射群組新增至 Windows 部署服務伺服器。

## <a name="syntax"></a>語法
```
wdsutil [Options] /add-ImageGroup imageGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|imagegroup:<Image group name>|指定要新增之映射群組的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要新增映射群組，請輸入下列其中一項：
```
wdsutil /add-ImageGroup imageGroup:ImageGroup2
wdsutil /verbose /add-Imagegroup imageGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil get-allimagegroups 命令](wdsutil-get-allimagegroups.md)
- [wdsutil get-imagegroup 命令](wdsutil-get-imagegroup.md)
- [wdsutil remove-imagegroup 命令](wdsutil-remove-imagegroup.md)
- [wdsutil 設定-imagegroup 命令](wdsutil-set-imagegroup.md)
