---
title: 新 G e
description: 新 G e 的參考文章，可從現有的開機映射建立新的捕獲映射。
ms.topic: reference
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e98b0b52df39f20b9c84a55aa96c569943b556e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038166"
---
# <a name="new-captureimage"></a>新 G e

從現有的開機映射建立新的捕獲映射。 「捕獲映射」是啟動 Windows 部署服務 capture 公用程式，而不是啟動安裝程式的開機映射。 當您開機 (的參照電腦，並以 Sysprep) 準備進入捕獲映射時，嚮導會建立參照電腦的安裝映射，並將它儲存為 Windows 映像 ( .wim) 檔。 您也可以將映射新增至媒體 (例如 CD、DVD 或 USB 磁片磁碟機) ，然後從該媒體將電腦開機。 在您建立安裝映像之後，可以將映像新增至將用於執行 PXE 開機部署的伺服器。 如需詳細資訊，請參閱建立映射 ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)) 。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /New-CaptureImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
        /FilePath:<File path and name>
        [/Name:<Name>]
        [/Description:<Description>]
        [/Overwrite:{Yes | No | Append}]
        [/UnattendFilePath:<File path>]
```

### <a name="parameters"></a>參數

|        參數         |                                                                                                                                                                                                                         描述                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： \<Server name> ] |                                                                                                                                       指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                        |
|   圖符\<Image name>   |                                                                                                                                                                                                         指定來源開機映射的名稱。                                                                                                                                                                                                         |
|   /Architecture： {x86    |                                                                                                                                                                                                                             ia64                                                                                                                                                                                                                             |
| [/Filename： \<Filename> ] |                                                                                                                                                                            如果映射無法依名稱唯一識別，您必須使用此選項來指定檔案名。                                                                                                                                                                            |
|    /DestinationImage     | 指定目的地映射的設定。 您可以使用下列選項來指定設定：</br>-/FilePath： \<File path and name> 設定新的捕獲映射的完整檔案路徑。</br>-[/Name： \<Name> ]-設定影像的顯示名稱。 如果未指定顯示名稱，將會使用來源影像的顯示名稱。</br>-[/Description： \<Description> ]-設定影像的描述。</br>-[/Overwrite： {Yes |

## <a name="examples"></a>範例

若要建立 capture 映射並將它命名為 WinPECapture，請輸入：
```
WDSUTIL /New-CaptureImage /Image:WinPE boot image /Architecture:x86 /DestinationImage /FilePath:C:\Temp\WinPECapture.wim
```
若要建立 capture 映射並套用指定的設定，請輸入：
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:WinPE boot image /Architecture:x64 /Filename:boot.wim
/DestinationImage /FilePath:\\Server\Share\WinPECapture.wim /Name:New WinPE image /Description:WinPE image with capture utility /Overwrite:No /UnattendFilePath:\\Server\Share\WDSCapture.inf
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)