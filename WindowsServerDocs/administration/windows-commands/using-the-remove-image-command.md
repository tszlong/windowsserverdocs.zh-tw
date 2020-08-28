---
title: 移除-映射
description: 移除映射的參考文章，這會從伺服器刪除影像。
ms.topic: reference
ms.assetid: ce5e2384-2264-4b22-92af-74eec8c10ae0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d720a924424840224afd1ecd88c48c4f9a9a1163
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023182"
---
# <a name="remove-image"></a>移除-映射

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從伺服器刪除影像。

## <a name="syntax"></a>語法
針對開機映射：
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
媒體：<Image name>|指定映像的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體媒體： {Boot &#124; Install}|指定映射的類型。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定映射的架構。 由於不同架構的不同開機映射可以有相同的映射名稱，因此指定架構值可確保將會移除正確的映射。|
|\mediaGroup： <Image group name> ]|指定包含映射的映射群組。 如果未指定映射組名，且伺服器上只有一個映射群組，則會使用該映射群組。 如果有一個以上的映射群組存在，您必須使用此選項來指定映射群組。|
|[/Filename： <File name> ]|如果映射無法依名稱唯一識別，您必須使用此選項來指定檔案名。|
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
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用新增映射命令](using-the-add-image-command.md) 
[使用複製映射命令](using-the-copy-image-command.md) 
[使用 Export-Image 命令](using-the-export-image-command.md) 
[使用取得映射命令](using-the-get-image-command.md) 
[使用取代映射命令](using-the-replace-image-command.md) 
[子命令：設定映射](subcommand-set-image.md)
