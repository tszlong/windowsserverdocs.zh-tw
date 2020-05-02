---
title: 取得-影像
description: 取得影像的參考主題，它會抓取影像的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 04cc7b8d90415e32be4103ef6c7f7b709c3550c3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719917"
---
# <a name="get-image"></a>取得-影像

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取影像的相關資訊。

## <a name="syntax"></a>語法
開機映射：
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
針對安裝映射：
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒介<Image name>|指定映像的名稱。|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體： {Boot &#124; 安裝}|指定映射的類型。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定映射的架構。 因為不同架構中的開機映射可能會有相同的映射名稱，所以指定架構值可確保傳回正確的映射。|
|[/Filename：<File name>]|如果無法以名稱唯一識別映射，您就必須使用這個選項來指定檔案名。|
|\mediaGroup：<Image group name>]|指定包含影像的映射群組。 如果未指定映射群組，而且伺服器上只存在一個映射群組，則會使用該群組。 如果伺服器上有一個以上的映射群組，您就必須使用這個參數來指定映射群組。|
## <a name="examples"></a>範例
若要取出開機映射的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86
wdsutil /verbose /Get-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
```
若要取出安裝映射的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-Imagmedia:Windows Vista with Officemediatype:Install
wdsutil /verbose /Get-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>其他參考
- [命令列語法索引鍵](command-line-syntax-key.md)
[Using the add-Image Command](using-the-add-image-command.md)
[：](subcommand-set-image.md)使用使用 [[匯出](using-the-export-image-command.md)
[Using the remove-Image Command](using-the-remove-image-command.md)

-映射] 命令的 [[複製映射](using-the-copy-image-command.md)
] 命令，透過使用 [映射[] 命令，](using-the-replace-image-command.md)透過 [圖像命令] 命令將
