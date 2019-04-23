---
title: 使用移除映像命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce5e2384-2264-4b22-92af-74eec8c10ae0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2aaf7d63d858045f9dd5df399c5f3f92b038bc2e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846699"
---
# <a name="using-the-remove-image-command"></a>使用移除映像命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

從伺服器刪除映像。
## <a name="syntax"></a>語法
針對開機映像：
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
```
安裝映像：
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<Filename>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定映像的名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
媒體類型: {開機&#124;安裝}|指定映像的類型。|
|/ 架構: {x86 &#124; ia64 &#124; x64}|指定映像的架構。 因為它可能會有不同的開機映像的相同映像名稱不同的架構中，指定架構值可確保正確的映像將會移除。|
|\mediaGroup:<Image group name>]|指定包含該映像的映像群組。 如果沒有映像群組名稱指定，且只能有一個映像群組存在於伺服器上，將會使用該映像群組。 有一個以上的映像群組時，您必須使用此選項來指定映像群組。|
|[/ Filename:<File name>]|如果映像無法依名稱來唯一識別，您必須使用此選項來指定檔案名稱。|
## <a name="BKMK_examples"></a>範例
若要移除的開機映像，請輸入：
```
wdsutil /remove-Imagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86
```
```
wdsutil /verbose /remove-Imagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim
```
若要移除的安裝映像，請輸入：
```
wdsutil /remove-Imagmedia:"Windows Vista with Officemediatype:Install
```
```
wdsutil /verbose /remove-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增映像命令](using-the-add-image-command.md)
[使用 [複製影像] 命令](using-the-copy-image-command.md)
[使用匯出映像命令](using-the-export-image-command.md)
[取得映像命令](using-the-get-image-command.md)
[使用 [取代影像] 命令](using-the-replace-image-command.md)
 [子命令： 設定影像](subcommand-set-image.md)
