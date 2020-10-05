---
title: wdsutil get-imagegroup
description: Wdsutil imagegroup 的參考文章，可取得映射群組和其中影像的相關資訊。
ms.topic: reference
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d40fef643ebbb8fd48f36fc048ddefc0b4b3229f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729900"
---
# <a name="wdsutil-get-imagegroup"></a>wdsutil get-imagegroup

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取影像群組和其內影像的相關資訊。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-ImageGroup ImageGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/ImageGroup：<Image group name>|指定映射群組的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/detailed]|傳回每個影像的影像中繼資料。 如果未使用此參數，預設行為是只傳回映射名稱、描述和檔案名。|
## <a name="examples"></a>範例
若要查看映射群組的相關資訊，請輸入：
```
wdsutil /Get-ImageGroup ImageGroup:ImageGroup1
```
若要查看包含中繼資料的資訊，請輸入：
```
wdsutil /verbose /Get-ImageGroup ImageGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-imagegroup 命令](wdsutil-add-imagegroup.md)
- [wdsutil get-allimagegroups 命令](wdsutil-get-allimagegroups.md)
- [wdsutil remove-imagegroup 命令](wdsutil-remove-imagegroup.md)
- [wdsutil 設定-imagegroup 命令](wdsutil-set-imagegroup.md)
