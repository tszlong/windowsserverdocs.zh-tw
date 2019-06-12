---
title: 使用新 CaptureImage 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 00f15ae7ee1a7ab1ac1f71599d2cae9bb51d921e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440411"
---
# <a name="using-the-new-captureimage-command"></a>使用新 CaptureImage 命令



從現有的開機映像中建立新的擷取映像。 擷取映像是指啟動 Windows 部署服務的開機映像擷取公用程式，而不是啟動安裝程式。 當您的參照電腦 （具有已經以 Sysprep 備妥） 開機成擷取映像時，精靈會建立安裝映像的參照電腦，並將它儲存為 Windows 映像 (.wim) 檔案。 您可以也將影像新增至媒體 （例如 CD、 DVD 或 USB 磁碟機），並從該媒體將電腦開機。 建立安裝映像之後，您可以將映像加入 PXE 開機部署的伺服器。 如需詳細資訊，請參閱 < 建立映像 ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311))。

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

## <a name="parameters"></a>參數

|        參數         |                                                                                                                                                                                                                         描述                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/ 伺服器：\<伺服器名稱 >] |                                                                                                                                       指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。                                                                                                                                        |
|   / 映像：\<映像名稱 >   |                                                                                                                                                                                                         指定的來源開機映像的名稱。                                                                                                                                                                                                         |
|   / 架構: {x 86    |                                                                                                                                                                                                                             ia64                                                                                                                                                                                                                             |
| [/ 檔案名稱：\<Filename>] |                                                                                                                                                                            如果映像無法依名稱來唯一識別，您必須使用此選項來指定檔案名稱。                                                                                                                                                                            |
|    /DestinationImage     | 指定目的端影像的設定。 您指定的設定，使用下列選項：</br>-   /FilePath:\<檔案路徑和名稱 > 設定新的擷取映像的完整檔案路徑。</br>-[/ 名稱：\<名稱 >]-設定的映像的顯示名稱。 如果未不指定任何顯示名稱，則會使用來源映像的顯示名稱。</br>-[/ 描述：\<描述 >]-設定映像的描述。</br>-[覆寫 /: {是 |

## <a name="BKMK_examples"></a>範例

若要建立擷取映像並將它命名為 WinPECapture.wim，輸入：
```
WDSUTIL /New-CaptureImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPECapture.wim"
```
若要建立擷取映像，並套用指定的設定，請輸入：
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim 
/DestinationImage /FilePath:"\\Server\Share\WinPECapture.wim" /Name:"New WinPE image" /Description:"WinPE image with capture utility" /Overwrite:No /UnattendFilePath:"\\Server\Share\WDSCapture.inf"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)