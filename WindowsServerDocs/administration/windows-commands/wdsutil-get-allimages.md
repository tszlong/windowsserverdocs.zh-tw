---
title: wdsutil get-allimages
description: Wdsutil allimages 的參考文章，可取得伺服器上所有影像的相關資訊。
ms.topic: reference
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fc4234e199d0eb39c3f8d94c31f2330f60cdc167
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729916"
---
# <a name="wdsutil-get-allimages"></a>wdsutil get-allimages

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
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-image 命令](wdsutil-add-image.md)
- [wdsutil 複製映射命令](wdsutil-copy-image.md)
- [wdsutil 匯出-image 命令](wdsutil-export-image.md)
- [wdsutil remove-image 命令](wdsutil-remove-image.md)
- [wdsutil 取代-image 命令](wdsutil-replace-image.md)
- [wdsutil 設定-image 命令](wdsutil-set-image.md)
