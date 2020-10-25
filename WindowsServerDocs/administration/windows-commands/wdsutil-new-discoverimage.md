---
title: 新 G e
description: 新 G e 的參考文章，可從現有的開機映射建立新的探索映射。
ms.topic: reference
ms.assetid: ede9fbbb-0bba-4309-8c21-3cc13e1dc3cd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 693b9866dbd19fa649622d5b97b3474351c96a35
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524253"
---
# <a name="new-discoverimage"></a>新 G e

從現有的開機映射建立新的探索映射。 探索映射是強制 Setup.exe 程式在 Windows 部署服務模式下啟動，然後探索 Windows 部署服務伺服器的開機映射。 這些映射通常是用來將映射部署到無法開機至 PXE 的電腦。 如需詳細資訊，請參閱建立映射 ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)) 。

## <a name="syntax"></a>語法

```
wdsutil [Options] /New-DiscoverImage [/Server:<Server name>]
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

### <a name="parameters"></a>參數

|        參數         |                                                                                                                                                                                                                                                                                                                                                                                                                       說明                                                                                                                                                                                                                                                                                                                                                                                                                       |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： \<Server name> ] |                                                                                                                                                                                                                                                                                                                                     指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                                                                                                                                                                                                                     |
|   圖符\<Image name>   |                                                                                                                                                                                                                                                                                                                                                                                                      指定來源開機映射的名稱。                                                                                                                                                                                                                                                                                                                                                                                                       |
|    /Architecture： {x86    |                                                                                                                                                                                                                                                                                                                                                                                                                          ia64                                                                                                                                                                                                                                                                                                                                                                                                                           |
| [/Filename： \<File name> ] |                                                                                                                                                                                                                                                                                                                                                                         如果映射無法依名稱唯一識別，您必須使用此選項來指定檔案名。                                                                                                                                                                                                                                                                                                                                                                          |
|    /DestinationImage     | 指定目的地映射的設定。 您可以使用下列選項來指定設定：</br>-/FilePath： < 檔案路徑和名稱>-設定新映射的完整檔案路徑。</br>-[/Name： \<Name> ]-設定影像的顯示名稱。 如果未指定顯示名稱，將會使用來源影像的顯示名稱。</br>-[/Description： \<Description> ]-設定影像的描述。</br>-[/WDSServer： \<Server name> ]-指定從指定映射開機的所有用戶端都必須聯絡才能下載安裝映射的伺服器名稱。 根據預設，啟動此映射的所有用戶端將會探索有效的 Windows 部署服務伺服器。 使用這個選項會略過探索功能，並強制開機的用戶端連接到指定的伺服器。</br>-[/Overwrite： {Yes |

## <a name="examples"></a>範例

若要從開機映射建立探索映射並將它命名為 WinPEDiscover，請輸入：
```
wdsutil /New-DiscoverImage /Image:WinPE boot image /Architecture:x86 /DestinationImage /FilePath:C:\Temp\WinPEDiscover.wim
```
若要從開機映射建立探索映射，並使用指定的設定將它命名為 WinPEDiscover，請輸入：
```
wdsutil /Verbose /Progress /New-DiscoverImage /Server:MyWDSServer
/Image:WinPE boot image /Architecture:x64 /Filename:boot.wim /DestinationImage /FilePath:\\Server\Share\WinPEDiscover.wim
/Name:New WinPE image /Description:WinPE image for WDS Client discovery /Overwrite:No
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)