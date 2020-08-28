---
title: AllImages
description: AllImages 的參考文章，可取得伺服器上所有影像的相關資訊。
ms.topic: reference
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4ebc0b36d832b6ce35168f6160b36c1c2ff896e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035986"
---
# <a name="get-allimages"></a>AllImages

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取伺服器上所有影像的相關資訊。

## <a name="syntax"></a>語法
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Show： {Boot &#124; 安裝 &#124; LegacyRis &#124; All}|-   **開機** 只會傳回開機映射。<br />-   **Install** 會傳回安裝映射，以及包含這些映射的映射群組的相關資訊。<br />-   **LegacyRis** 只會傳回 (RIS) 映射的遠端安裝服務。<br />-   **全都** 會傳回開機映射資訊、安裝映射資訊 (包括映射群組) 的相關資訊，以及 RIS 映射資訊。|
|[/detailed]|表示應該傳回每個影像的所有影像中繼資料。 如果未使用此選項，預設行為是只傳回映射名稱、描述和檔案名。|
## <a name="examples"></a>範例
若要查看影像的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用新增映射命令](using-the-add-image-command.md) 
[使用複製映射命令](using-the-copy-image-command.md) 
[使用 Export-Image 命令](using-the-export-image-command.md) 
[使用移除映射命令](using-the-remove-image-command.md) 
[使用取代映射命令](using-the-replace-image-command.md) 
[子命令：設定映射](subcommand-set-image.md)
