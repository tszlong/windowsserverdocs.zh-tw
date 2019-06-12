---
title: 使用新 DiscoverImage 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4a017e3090ec05bbcd7984e630cdb3670a35bd27
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440427"
---
# <a name="using-the-new-discoverimage-command"></a>使用新 DiscoverImage 命令



從現有的開機映像中建立新的探索映像。 探索映像是強制 Setup.exe 程式，以在 Windows 部署服務模式中啟動，並接著就會探索 Windows 部署服務伺服器的開機映像。 通常這些映像會用來將映像部署到不能以 PXE 開機的電腦。 如需詳細資訊，請參閱 < 建立映像 ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311))。

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
| [/ 伺服器：\<伺服器名稱 >] |                                                                                                                                                                                                                                                                                                                                     指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。                                                                                                                                                                                                                                                                                                                                     |
|   / 映像：\<映像名稱 >   |                                                                                                                                                                                                                                                                                                                                                                                                      指定的來源開機映像的名稱。                                                                                                                                                                                                                                                                                                                                                                                                       |
|    / 架構: {x86    |                                                                                                                                                                                                                                                                                                                                                                                                                          ia64                                                                                                                                                                                                                                                                                                                                                                                                                           |
| [/ Filename:\<檔案名稱 >] |                                                                                                                                                                                                                                                                                                                                                                         如果映像無法依名稱來唯一識別，您必須使用此選項來指定檔案名稱。                                                                                                                                                                                                                                                                                                                                                                          |
|    /DestinationImage     | 指定目的端影像的設定。 您可以指定使用下列選項的設定：</br>-/FilePath: < 檔案路徑和名稱 >-新的映像的集完整檔案路徑。</br>-[/Name:\<名稱 >]-設定的映像的顯示名稱。 如果未不指定任何顯示名稱，則會使用來源映像的顯示名稱。</br>-[/ 描述：\<描述 >]-設定映像的描述。</br>-[/ WDSServer:\<伺服器名稱 >]-指定從指定的映像開機的所有用戶端應聯繫以下載的安裝映像的伺服器名稱。 根據預設，此映像開機的所有用戶端會探索有效的 Windows 部署服務伺服器。 使用此選項時，會略過探索功能，並強制開機的用戶端連絡指定的伺服器。</br>-[覆寫 /: {是 |

## <a name="BKMK_examples"></a>範例

若要建立探索映像從開機映像，並將它命名為 WinPEDiscover.wim，輸入：
```
WDSUTIL /New-DiscoverImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPEDiscover.wim"
```
若要建立探索映像從開機映像，並將它命名 WinPEDiscover.wim 使用指定的設定，輸入：
```
WDSUTIL /Verbose /Progress /New-DiscoverImage /Server:MyWDSServer
/Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim /DestinationImage /FilePath:"\\Server\Share\WinPEDiscover.wim" 
/Name:"New WinPE image" /Description:"WinPE image for WDS Client discovery" /Overwrite:No
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)