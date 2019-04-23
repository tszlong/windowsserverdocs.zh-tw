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
ms.openlocfilehash: 7d9f402acb9904624bdb4193a4306d57b104eda8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888609"
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

|參數|描述|
|---------|-----------|
|[/ 伺服器：\<伺服器名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|/ 映像：\<映像名稱 >|指定的來源開機映像的名稱。|
|/ 架構: {x 86 | ia64 | x64}|指定要使用的映像的架構。 因為您可以將不同的開機映像的相同映像名稱在不同的架構，可指定這可確保正確的映像。|
|[/ 檔案名稱：\<Filename>]|如果映像無法依名稱來唯一識別，您必須使用此選項來指定檔案名稱。|
|/DestinationImage|指定目的端影像的設定。 您指定的設定，使用下列選項：</br>-   /FilePath:\<檔案路徑和名稱 > 設定新的擷取映像的完整檔案路徑。</br>-[/ 名稱：\<名稱 >]-設定的映像的顯示名稱。 如果未不指定任何顯示名稱，則會使用來源映像的顯示名稱。</br>-[/ 描述：\<描述 >]-設定映像的描述。</br>-[覆寫 /: {是 | 否 | Append}]-決定是否在指定的檔案 **/DestinationImage**應該覆寫 /FilePath 端已有同名的另一個檔案。 **是**覆寫現有的檔案。 **否**（預設值） 會導致錯誤發生，如果已經存在具有相同名稱的另一個檔案。 **附加**將產生的映像附加為現有的.wim 檔案內的新映像。</br>-   [/UnattendFilePath:\<檔案路徑 >]-設定的完整路徑和無人看管的映像擷取檔案名稱。|

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