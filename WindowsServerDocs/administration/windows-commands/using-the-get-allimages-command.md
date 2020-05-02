---
title: AllImages
description: AllImages 的參考主題，它會抓取伺服器上所有影像的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1f32a1789b22d04b7b61979d0ea49d91f0cf157
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720022"
---
# <a name="get-allimages"></a>AllImages

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取伺服器上所有影像的相關資訊。

## <a name="syntax"></a>語法
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Show： {Boot &#124; Install &#124; LegacyRis &#124; All}|-   **開機**只會傳回開機映射。<br />-   **Install**會傳回安裝映射，以及包含它們之映射群組的相關資訊。<br />-   **LegacyRis**只會傳回遠端安裝服務（RIS）映射。<br />-   **全部都會**傳回開機映射資訊、安裝映射資訊（包括映射群組的相關資訊），以及 RIS 映射資訊。|
|[/detailed]|表示應該傳回每個影像中的所有影像中繼資料。 如果未使用此選項，則預設行為是只傳回映射名稱、描述和檔案名。|
## <a name="examples"></a>範例
若要查看影像的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>其他參考
- [命令列語法索引鍵](command-line-syntax-key.md)
[Using the add-Image Command](using-the-add-image-command.md)
[：](subcommand-set-image.md)使用使用 [[匯出](using-the-export-image-command.md)
[Using the remove-Image Command](using-the-remove-image-command.md)

-映射] 命令的 [[複製映射](using-the-copy-image-command.md)
] 命令，透過使用 [映射[] 命令，](using-the-replace-image-command.md)透過 [圖像命令] 命令將
