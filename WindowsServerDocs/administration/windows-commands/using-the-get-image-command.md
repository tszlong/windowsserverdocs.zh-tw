---
title: 使用 get 映像命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b78f4ed9352c21bf6de19136a625a4f4fe7ac5f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877439"
---
# <a name="using-the-get-image-command"></a>使用 get 映像命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

擷取映像的相關資訊。
## <a name="syntax"></a>語法
針對開機映像：
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
安裝映像：
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定映像的名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
媒體類型: {開機&#124;安裝}|指定映像的類型。|
|/ 架構: {x86 &#124; ia64 &#124; x64}|指定映像的架構。 因為可能有不同的架構中的開機映像的相同映像名稱，指定架構值可確保會傳回正確的映像。|
|[/ Filename:<File name>]|如果映像無法依名稱來唯一識別，您必須使用此選項來指定檔案名稱。|
|\mediaGroup:<Image group name>]|指定包含該映像的映像群組。 如果未指定任何映像群組，而且只有一個映像群組存在於伺服器上，將會使用該群組。 如果伺服器上有一個以上的映像群組，您必須使用這個參數來指定映像群組。|
## <a name="BKMK_examples"></a>範例
若要擷取開機映像的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86
wdsutil /verbose /Get-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
```
若要擷取安裝映像的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-Imagmedia:"Windows Vista with Officemediatype:Install
wdsutil /verbose /Get-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增映像命令](using-the-add-image-command.md)
[使用 [複製影像] 命令](using-the-copy-image-command.md)
[使用匯出映像命令](using-the-export-image-command.md)
[移除映像命令](using-the-remove-image-command.md)
[使用 [取代影像] 命令](using-the-replace-image-command.md)
 [子命令： 設定影像](subcommand-set-image.md)
