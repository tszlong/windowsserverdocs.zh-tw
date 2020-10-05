---
title: wdsutil 移除-映射
description: Wdsutil remove-image 的參考文章，它會從伺服器刪除影像。
ms.topic: reference
ms.assetid: ce5e2384-2264-4b22-92af-74eec8c10ae0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f1d955f54f345b6d2832eabe2334dd77f32633e7
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730512"
---
# <a name="wdsutil-remove-image"></a>wdsutil 移除-映射

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從伺服器刪除影像。

## <a name="syntax"></a>Syntax
針對開機映射：
```
wdsutil [Options] /remove-Image:<Image name> [/Server:<Server name> type:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
```
針對安裝映射：
```
wdsutil [Options] /remove-image:<Image name> [/Server:<Server name> type:Install ImageGroup:<Image group name>] [/Filename:<Filename>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
| /remove-image:<Image name>|指定映像的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體媒體： {Boot &#124; Install}|指定映射的類型。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定映射的架構。 由於不同架構的不同開機映射可以有相同的映射名稱，因此指定架構值可確保將會移除正確的映射。|
|\ImageGroup： <Image group name> ]|指定包含映射的映射群組。 如果未指定映射組名，且伺服器上只有一個映射群組，則會使用該映射群組。 如果有一個以上的映射群組存在，您必須使用此選項來指定映射群組。|
|[/Filename： <File name> ]|如果映射無法依名稱唯一識別，您必須使用此選項來指定檔案名。|
## <a name="examples"></a>範例
若要移除開機映射，請輸入：
```
wdsutil /remove-Imagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86
```
```
wdsutil /verbose /remove-Image:WinPE Boot Image /Server:MyWDSServer type:Boot /Architecture:x64 /Filename:boot.wim
```
若要移除安裝映射，請輸入：
```
wdsutil /remove-Image:Windows Vista with Officemediatype:Install
```
```
wdsutil /verbose /remove-Image:Windows Vista with Office /Server:MyWDSServemediatype:Instal ImageGroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-image 命令](wdsutil-add-image.md)
- [wdsutil 複製映射命令](wdsutil-copy-image.md)
- [wdsutil 匯出-image 命令](wdsutil-export-image.md)
- [wdsutil 取得映射命令](wdsutil-get-image.md)
- [wdsutil 取代-image 命令](wdsutil-replace-image.md)
- [wdsutil 設定-image 命令](wdsutil-set-image.md)
