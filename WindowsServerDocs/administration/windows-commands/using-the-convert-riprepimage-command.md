---
title: 使用轉換 RiprepImage 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9b41b6dcc52c3e6700d1d18c61eceea8b990ecdf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440586"
---
# <a name="using-the-convert-riprepimage-command"></a>使用轉換 RiprepImage 命令



將現有的遠端安裝準備 (RIPrep) 映像轉換成 Windows 映像 (.wim) 格式。

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
| / FilePath:\<檔路徑和名稱 > |                                                                                                                                                                                                       指定對應至 RIPrep 映像的.sif 檔案的完整路徑和檔案名稱。 此檔案通常稱為 Riprep.sif，而且位於 \Templates 資料夾的子資料夾，其中包含 RIPrep 映像。                                                                                                                                                                                                       |
|        /DestinationImage        | 指定目的地映像中，使用下列選項的設定。</br>-/FilePath:\<檔路徑和名稱 >-設定新的檔案的完整檔案路徑。 例如: **C:\Temp\convert.wim**</br>-[/Name:\<名稱 >]-設定的映像的顯示名稱。 如果未不指定任何顯示名稱，則會使用來源映像的顯示名稱。</br>-[/ 描述：\<描述 >]-設定映像的描述。</br>-[/InPlace]-指定轉換應該會在原始的 RIPrep 映像上，而不是在一份原始影像，這是預設行為的發生。</br>-[覆寫 /: {是 |

## <a name="BKMK_examples"></a>範例

若要將指定的 RIPrep.sif 映像轉換成 RIPREP.wim 中，輸入：
```
WDSUTIL /Convert-RiPrepImage /FilePath:"R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif" /DestinationImage
/FilePath:"C:\Temp\RIPREP.wim"
```
若要將指定的 RIPrep.sif 映像轉換成 RIPREP.wim，具有指定的名稱和描述，並覆寫它的新檔案的檔案已存在時，請輸入：
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:"\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif"
/DestinationImage /FilePath:"\\Server\Share\RIPREP.wim"
/Name:"WindowsXP image" /Description:"Converted RIPREP image of WindowsXP"
/Overwrite:Append
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)