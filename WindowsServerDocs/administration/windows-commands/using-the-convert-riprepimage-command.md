---
title: 使用 convert-RiprepImage 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0b5e75a4148359db5088e7ff60d29b8d157309d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363655"
---
# <a name="using-the-convert-riprepimage-command"></a>使用 convert-RiprepImage 命令



將現有的遠端安裝準備（RIPrep）映射轉換成 Windows 映像（.wim）格式。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Convert-RIPrepImage /FilePath:<File path and name>
     /DestinationImage
         /FilePath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
         [/InPlace]
         [/Overwrite:{Yes | No | Append}]
```

## <a name="parameters"></a>參數

|            參數            |                                                                                                                                                                                                                                                                                                               描述                                                                                                                                                                                                                                                                                                                |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /FilePath： \<File 路徑和名稱 > |                                                                                                                                                                                                       指定對應于 RIPrep 映射之 .sif 檔案的完整路徑和檔案名。 這個檔案通常稱為 Riprep，在包含 RIPrep 映射的資料夾的 \Templates 子資料夾中找到。                                                                                                                                                                                                       |
|        /DestinationImage        | 使用下列選項指定目的地映射的設定。</br>-/FilePath： \<File 路徑和名稱 >-設定新檔案的完整檔案路徑。 例如: **C:\Temp\convert.wim**</br>-[/Name： \<Name >]-設定影像的顯示名稱。 如果未指定顯示名稱，則會使用來源影像的顯示名稱。</br>-[/Description：\<Description >]-設定影像的描述。</br>-[/InPlace]-指定應該在原始 RIPrep 映射上進行轉換，而不是在原始映射的複本上進行，這是預設行為。</br>-[/Overwrite： {Yes |

## <a name="BKMK_examples"></a>典型

若要將指定的 RIPrep 映射轉換為 RIPREP .wim，請輸入：
```
WDSUTIL /Convert-RiPrepImage /FilePath:"R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif" /DestinationImage
/FilePath:"C:\Temp\RIPREP.wim"
```
若要將指定的 RIPrep 映射轉換為具有指定之名稱和描述的 RIPREP .wim，並以新的檔案覆寫它（如果檔案已經存在），請輸入：
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:"\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif"
/DestinationImage /FilePath:"\\Server\Share\RIPREP.wim"
/Name:"WindowsXP image" /Description:"Converted RIPREP image of WindowsXP"
/Overwrite:Append
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)