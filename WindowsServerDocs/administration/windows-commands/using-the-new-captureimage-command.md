---
title: 新增-G e
description: G e 的參考文章，它會從現有的開機映射建立新的 capture 映射。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec9da39ccef5efcea1792556ef8335bbf63c5fd6
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932148"
---
# <a name="new-captureimage"></a>新增-G e

從現有的開機映射建立新的 capture 映射。 「捕獲映射」是啟動 Windows 部署服務 capture 公用程式，而不是啟動安裝程式的開機映射。 當您將參照電腦（已使用 Sysprep 準備）開機成捕獲映射時，嚮導會建立參照電腦的安裝映射，並將它儲存為 Windows 映像（.wim）檔案。 您也可以將映射新增至媒體（例如 CD、DVD 或 USB 磁片磁碟機），然後從該媒體將電腦開機。 在您建立安裝映像之後，可以將映像新增至將用於執行 PXE 開機部署的伺服器。 如需詳細資訊，請參閱建立影像（ [https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311) ）。

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

|        參數         |                                                                                                                                                                                                                         說明                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： \<Server name> ] |                                                                                                                                       指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                        |
|   包\<Image name>   |                                                                                                                                                                                                         指定來源開機映射的名稱。                                                                                                                                                                                                         |
|   /Architecture： {x86    |                                                                                                                                                                                                                             ia64                                                                                                                                                                                                                             |
| [/Filename： \<Filename> ] |                                                                                                                                                                            如果無法以名稱唯一識別映射，您就必須使用這個選項來指定檔案名。                                                                                                                                                                            |
|    /DestinationImage     | 指定目的地映射的設定。 您可以使用下列選項來指定設定：</br>-/FilePath： \<File path and name> 設定新的 capture 映射的完整檔案路徑。</br>-[/Name： \<Name> ]-設定影像的顯示名稱。 如果未指定顯示名稱，則會使用來源影像的顯示名稱。</br>-[/Description： \<Description> ]-設定影像的描述。</br>-[/Overwrite： {Yes |

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