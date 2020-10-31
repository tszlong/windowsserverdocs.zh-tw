---
title: wdsutil 匯出-影像
description: Wdsutil export-image 命令的參考文章，此命令會將現有映射從映射存放區匯出至另一個 Windows 映像 ( .wim) 檔。
ms.topic: reference
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8c3c390e43cc8707d8d94ade757130276b5f2753
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071040"
---
# <a name="wdsutil-export-image"></a>wdsutil 匯出-影像

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將映射存放區中的現有映射匯出到另一個 Windows 映像 ( .wim) 檔。

## <a name="syntax"></a>Syntax

針對開機映射：

```
wdsutil [options] /Export-Image image:<Image name> [/Server:<Servername>]
    imagetype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
     /DestinationImage
         /Filepath:<Filepath and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No}]
```

針對安裝映射：

```
wdsutil [options] /Export-Image image:<Image name> [/Server:<Servername>]
    imagetype:Install imageGroup:<Image group name>]
     [/Filename:<Filename>]
     /DestinationImage
         /Filepath:<Filepath and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No | append}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| 圖像：`<Imagename>` | 指定要匯出之映射的名稱。 |
| [/Server： `<Servername>` ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| imagetype`{Boot|Install}` | 指定要匯出的影像類型。 |
| \imageGroup： `<Image group name>` ] | 指定包含要匯出之映射的映射群組。 如果未指定映射組名，且伺服器上只有一個映射群組，則預設會使用該映射群組。 如果伺服器上有一個以上的映射群組，就必須指定映射群組。 |
| 之上`{x86|ia64|x64}` | 指定要匯出之映射的架構。 由於不同架構中的開機映射可以有相同的映射名稱，因此指定架構值可確保會傳回正確的映射。 |
| [/Filename： `<Filename>` ] | 如果映射無法依名稱唯一識別，則必須指定檔案名。 |
| /DestinationImage | 指定目的地映射的設定。 您可以使用下列選項來指定這些設定：<ul><li>`/Filepath:<Filepath and name>` -指定新映射的完整檔案路徑。</li><li>`[/Name:<Name>]` -設定影像的顯示名稱。 如果未指定名稱，則會使用來源影像的顯示名稱。</li><li>`[/Description: <Description>]` -設定影像的描述。</li></ul> |
| [/Overwrite： `{Yes|No|append}` ] | 判斷如果已有同名的現有檔案存在於/Filepath.， **/DestinationImage** 選項中指定的檔案是否會覆寫 [ **是]** 選項會覆寫現有的檔案，[ **否** ] 選項 (預設值) 如果已經有同名的檔案，則會導致錯誤發生，而且 [ **附加** ] 選項會導致產生的映射附加為現有 .wim 檔案內的新映射。 |

## <a name="examples"></a>範例

若要匯出開機映射，請輸入下列其中一項：

```
wdsutil /Export-Image image:WinPE boot image imagetype:Boot /Architecture:x86 /DestinationImage /Filepath:C:\temp\boot.wim
```

```
wdsutil /verbose /Progress /Export-Image image:WinPE boot image /Server:MyWDSServer imagetype:Boot /Architecture:x64 /Filename:boot.wim /DestinationImage /Filepath:\\Server\Share\ExportImage.wim /Name:Exported WinPE image /Description:WinPE Image from WDS server /Overwrite:Yes
```

若要匯出安裝映射，請輸入下列其中一項：

```
wdsutil /Export-Image image:Windows Vista with Office imagetype:Install /DestinationImage /Filepath:C:\Temp\Install.wim
```

```
wdsutil /verbose /Progress /Export-Image image:Windows Vista with Office /Server:MyWDSServer imagetype:Instal imageGroup:ImageGroup1 /Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:Exported Windows image /Description:Windows Vista image from WDS server /Overwrite:append
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil add-image 命令](wdsutil-add-image.md)

- [wdsutil 複製映射命令](wdsutil-copy-image.md)

- [wdsutil 取得映射命令](wdsutil-get-image.md)

- [wdsutil remove-image 命令](wdsutil-remove-image.md)

- [wdsutil 取代-image 命令](wdsutil-replace-image.md)

- [wdsutil 設定-image 命令](wdsutil-set-image.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)