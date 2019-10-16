---
title: 使用 AllImages 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5122a5660031d503795715c0005b404f910d6626
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363505"
---
# <a name="using-the-get-allimages-command"></a>使用 AllImages 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取伺服器上所有影像的相關資訊。
## <a name="syntax"></a>語法
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Show： {Boot &#124; Install &#124; LegacyRis &#124; All}|-   **開機**只會傳回開機映射。<br />-   **安裝**會傳回安裝映射，以及包含它們之映射群組的相關資訊。<br />-   **LegacyRis**只會傳回遠端安裝服務（RIS）映射。<br />-   **全都**會傳回開機映射資訊、安裝映射資訊（包括映射群組的相關資訊），以及 RIS 映射資訊。|
|[/detailed]|表示應該傳回每個影像中的所有影像中繼資料。 如果未使用此選項，則預設行為是只傳回映射名稱、描述和檔案名。|
## <a name="BKMK_examples"></a>典型
若要查看影像的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
 [使用新增映射命令](using-the-add-image-command.md)
[使用複製映射命令](using-the-copy-image-command.md)
，[使用匯出影像命令](using-the-export-image-command.md)
[使用移除映射命令](using-the-remove-image-command.md)
[使用取代-影像命令](using-the-replace-image-command.md)
[子命令：設定-影像](subcommand-set-image.md)
