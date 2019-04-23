---
title: 使用 get AllImages 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 57b81dd3dd3a24876c4401e80d08130ed5243888
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872559"
---
# <a name="using-the-get-allimages-command"></a>使用 get AllImages 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

擷取伺服器上的所有映像的相關資訊。
## <a name="syntax"></a>語法
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|/ 顯示: {開機&#124;安裝&#124;LegacyRis&#124;所有}|-   **開機**傳回只有開機映像。<br />-   **安裝**傳回安裝映像以及包含這些映像群組的相關資訊。<br />-   **LegacyRis**傳回只有遠端安裝服務 (RIS) 映像。<br />-   **所有**傳回開機映像資訊、 安裝映像資訊 （包括映像群組的相關資訊） 和 RIS 映像資訊。|
|[/ 詳細]|表示應該傳回所有的映像中繼資料，從每個映像。 如果未使用此選項，預設行為是傳回僅映像名稱、 描述和檔案名稱。|
## <a name="BKMK_examples"></a>範例
若要檢視有關映像的資訊，請輸入下列其中一項：
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增映像命令](using-the-add-image-command.md)
[使用 [複製影像] 命令](using-the-copy-image-command.md)
[使用匯出映像命令](using-the-export-image-command.md)
[移除映像命令](using-the-remove-image-command.md)
[使用 [取代影像] 命令](using-the-replace-image-command.md)
 [子命令： 設定影像](subcommand-set-image.md)
