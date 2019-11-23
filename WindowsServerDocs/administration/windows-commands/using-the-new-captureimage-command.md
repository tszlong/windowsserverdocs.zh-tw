---
title: 使用 G e 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb48cb76ef99eac51b862a5e1a3d1999a1cfc89d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363055"
---
# <a name="using-the-new-captureimage-command"></a>使用 G e 命令



從現有的開機映射建立新的 capture 映射。 「捕獲映射」是啟動 Windows 部署服務 capture 公用程式，而不是啟動安裝程式的開機映射。 當您將參照電腦（已使用 Sysprep 準備）開機成捕獲映射時，嚮導會建立參照電腦的安裝映射，並將它儲存為 Windows 映像（.wim）檔案。 您也可以將映射新增至媒體（例如 CD、DVD 或 USB 磁片磁碟機），然後從該媒體將電腦開機。 建立安裝映射之後，您可以將映射新增至伺服器以進行 PXE 開機部署。 如需詳細資訊，請參閱建立映射（[https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)）。

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

## <a name="parameters"></a>Parameters

|        參數         |                                                                                                                                                                                                                         描述                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server：\<伺服器名稱 >] |                                                                                                                                       指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                                                        |
|   /Image：\<映射名稱 >   |                                                                                                                                                                                                         指定來源開機映射的名稱。                                                                                                                                                                                                         |
|   /Architecture： {x86    |                                                                                                                                                                                                                             ia64                                                                                                                                                                                                                             |
| [/Filename： \<Filename >] |                                                                                                                                                                            如果無法以名稱唯一識別映射，您就必須使用這個選項來指定檔案名。                                                                                                                                                                            |
|    /DestinationImage     | 指定目的地映射的設定。 您可以使用下列選項來指定設定：</br>-/FilePath： \<檔案路徑和名稱 > 設定新捕獲映射的完整檔案路徑。</br>-[/Name： \<名稱 >]-設定影像的顯示名稱。 如果未指定顯示名稱，則會使用來源影像的顯示名稱。</br>-[/Description： \<Description >]-設定影像的描述。</br>-[/Overwrite： {Yes |

## <a name="BKMK_examples"></a>典型

若要建立 capture 映射並將它命名為 WinPECapture，請輸入：
```
WDSUTIL /New-CaptureImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPECapture.wim"
```
若要建立 capture 映射並套用指定的設定，請輸入：
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim 
/DestinationImage /FilePath:"\\Server\Share\WinPECapture.wim" /Name:"New WinPE image" /Description:"WinPE image with capture utility" /Overwrite:No /UnattendFilePath:"\\Server\Share\WDSCapture.inf"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)