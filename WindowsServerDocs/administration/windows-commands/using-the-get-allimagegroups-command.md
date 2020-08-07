---
title: AllImageGroups
description: AllImageGroups 的參考文章，它會抓取伺服器上所有映射群組的相關資訊，以及這些映射群組中的所有映射。
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 523c761df06d11f6c1af5488370b540af9bc7326
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879824"
---
# <a name="get-allimagegroups"></a>AllImageGroups

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取伺服器上所有映射群組的相關資訊，以及這些映射群組中的所有映射。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 (FQDN) 的完整功能變數名稱。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/detailed]|傳回每個影像中的影像中繼資料。 如果未使用此參數，則預設行為是只傳回映射名稱、描述和每個影像的檔案名。|
## <a name="examples"></a>範例
若要查看映射群組的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 ImageGroup 命令](using-the-add-imagegroup-command.md) 
[使用 ImageGroup 命令](using-the-get-imagegroup-command.md) 
[使用 ImageGroup 命令](using-the-remove-imagegroup-command.md) 
[子命令： set-ImageGroup](subcommand-set-imagegroup.md)
