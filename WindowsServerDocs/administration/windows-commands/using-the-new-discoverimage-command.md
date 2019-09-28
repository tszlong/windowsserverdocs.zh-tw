---
title: 使用 D g e 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ede9fbbb-0bba-4309-8c21-3cc13e1dc3cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b17777eb4d8541ce5669ee6becbd51e2b9da236
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362986"
---
# <a name="using-the-new-discoverimage-command"></a>使用 D g e 命令



從現有的開機映射建立新的探索映射。 探索映射是強制 Setup.exe 程式以 Windows 部署服務模式啟動，然後探索 Windows 部署服務伺服器的開機映射。 這些映射通常是用來將映射部署到無法開機至 PXE 的電腦。 如需詳細資訊，請參閱建立映射（[https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)）。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /New-DiscoverImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
         /FilePath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
         [/WDSServer:<Server name>]
         [/Overwrite:{Yes | No | Append}]
```

## <a name="parameters"></a>參數

|        參數         |                                                                                                                                                                                                                                                                                                                                                                                                                       描述                                                                                                                                                                                                                                                                                                                                                                                                                       |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： \<Server 名稱 >] |                                                                                                                                                                                                                                                                                                                                     指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                                                                                                                                                                                                     |
|   /Image： @no__t 0Image 名稱 >   |                                                                                                                                                                                                                                                                                                                                                                                                      指定來源開機映射的名稱。                                                                                                                                                                                                                                                                                                                                                                                                       |
|    /Architecture： {x86    |                                                                                                                                                                                                                                                                                                                                                                                                                          ia64                                                                                                                                                                                                                                                                                                                                                                                                                           |
| [/Filename： \<File 名稱 >] |                                                                                                                                                                                                                                                                                                                                                                         如果無法以名稱唯一識別映射，您就必須使用這個選項來指定檔案名。                                                                                                                                                                                                                                                                                                                                                                          |
|    /DestinationImage     | 指定目的地映射的設定。 您可以使用下列選項來指定設定：</br>-/FilePath： < 檔案路徑和名稱 >-設定新映射的完整檔案路徑。</br>-[/Name： \<Name >]-設定影像的顯示名稱。 如果未指定顯示名稱，則會使用來源影像的顯示名稱。</br>-[/Description：\<Description >]-設定影像的描述。</br>-[/WDSServer：\<Server name >]-指定從指定的映射開機的所有用戶端都應連線的伺服器名稱，以下載安裝映射。 根據預設，所有啟動此映射的用戶端都會發現有效的 Windows 部署服務伺服器。 使用此選項會略過探索功能，並強制開機用戶端與指定的伺服器連線。</br>-[/Overwrite： {Yes |

## <a name="BKMK_examples"></a>典型

若要從開機映射建立探索映射，並將其命名為 WinPEDiscover，請輸入：
```
WDSUTIL /New-DiscoverImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPEDiscover.wim"
```
若要從開機映射建立探索映射，並使用指定的設定將其命名為 WinPEDiscover，請輸入：
```
WDSUTIL /Verbose /Progress /New-DiscoverImage /Server:MyWDSServer
/Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim /DestinationImage /FilePath:"\\Server\Share\WinPEDiscover.wim" 
/Name:"New WinPE image" /Description:"WinPE image for WDS Client discovery" /Overwrite:No
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)