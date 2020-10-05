---
title: wdsutil get-影像
description: Wdsutil 取得映射的參考文章，可抓取影像的相關資訊。
ms.topic: reference
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0f6cfcf3783ef49f2dd57941ae9a2ae4feb3742e
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729898"
---
# <a name="wdsutil-get-image"></a>wdsutil get-影像

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取影像的相關資訊。

## <a name="syntax"></a>Syntax
針對開機映射：
```
wdsutil [Options] /Get-Image image:<Image name> [/Server:<Server name> imagetype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
針對安裝映射：
```
wdsutil [Options] /Get-image image:<Image name> [/Server:<Server name> imagetype:Install imagegroup:<Image group name>] [/Filename:<File name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
| 圖符<Image name>|指定映像的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
| imagetype： {Boot &#124; Install}|指定映射的類型。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定映射的架構。 由於不同架構中的開機映射可以有相同的映射名稱，因此指定架構值可確保傳回正確的映射。|
|[/Filename： <File name> ]|如果映射無法依名稱唯一識別，您必須使用此選項來指定檔案名。|
|\imagegroup： <Image group name> ]|指定包含映射的映射群組。 如果未指定映射群組，且伺服器上只有一個映射群組，則會使用該群組。 如果伺服器上有一個以上的映射群組，您必須使用此參數來指定映射群組。|
## <a name="examples"></a>範例
若要取出開機映射的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-Image image:WinPE boot imagetype:Boot /Architecture:x86
wdsutil /verbose /Get-Image image:WinPE boot image /Server:MyWDSServer imagetype:Boot /Architecture:x86 /Filename:boot.wim
```
若要取得安裝映射的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-Image:Windows Vista with Office imagetype:Install
wdsutil /verbose /Get-Image:Windows Vista with Office /Server:MyWDSServer imagetype:Install imagegroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-image 命令](wdsutil-add-image.md)
- [wdsutil 複製映射命令](wdsutil-copy-image.md)
- [wdsutil 匯出-image 命令](wdsutil-export-image.md)
- [wdsutil remove-image 命令](wdsutil-remove-image.md)
- [wdsutil 取代-image 命令](wdsutil-replace-image.md)
- [wdsutil 設定-image 命令](wdsutil-set-image.md)
