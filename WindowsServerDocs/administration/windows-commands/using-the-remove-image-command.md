---
title: 移除-影像
description: 移除映射的參考主題，這會從伺服器中刪除影像。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce5e2384-2264-4b22-92af-74eec8c10ae0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 770c8487bcfe0cba28bffcd32a05285d904ba21c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720365"
---
# <a name="remove-image"></a>移除-影像

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從伺服器刪除影像。

## <a name="syntax"></a>語法
開機映射：
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
```
針對安裝映射：
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<Filename>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒介<Image name>|指定映像的名稱。|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體： {Boot &#124; 安裝}|指定映射的類型。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定映射的架構。 因為不同架構中的不同開機映射可能會有相同的映射名稱，所以指定架構值可確保移除正確的映射。|
|\mediaGroup：<Image group name>]|指定包含影像的映射群組。 如果未指定映射組名，且伺服器上只存在一個映射群組，則會使用該映射群組。 如果有一個以上的映射群組存在，您就必須使用此選項來指定映射群組。|
|[/Filename：<File name>]|如果無法以名稱唯一識別映射，您就必須使用這個選項來指定檔案名。|
## <a name="examples"></a>範例
若要移除開機映射，請輸入：
```
wdsutil /remove-Imagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86
```
```
wdsutil /verbose /remove-Imagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim
```
若要移除安裝映射，請輸入：
```
wdsutil /remove-Imagmedia:Windows Vista with Officemediatype:Install
```
```
wdsutil /verbose /remove-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>其他參考
- [命令列語法索引鍵](command-line-syntax-key.md)
[Using the add-Image Command](using-the-add-image-command.md)
[：](subcommand-set-image.md)使用使用 [[匯出](using-the-export-image-command.md)
[Using the get-Image Command](using-the-get-image-command.md)

-映射] 命令的 [[複製映射](using-the-copy-image-command.md)
] 命令，[透過使用 [](using-the-replace-image-command.md)映射] 命令，透過 [
