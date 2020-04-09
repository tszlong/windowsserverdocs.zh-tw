---
title: 轉換-RiprepImage
description: RiprepImage 的 Windows 命令主題，可將現有的遠端安裝準備（RIPrep）映射轉換成 Windows 映像（.wim）格式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01e33580a6d2da55df15fabde70697c22f894f7c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831811"
---
# <a name="convert-riprepimage"></a>轉換-RiprepImage

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

### <a name="parameters"></a>參數

|            參數            |                                                                                                                                                                                                                                                                                                               描述                                                                                                                                                                                                                                                                                                                |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /FilePath：\<檔案路徑和名稱 > |                                                                                                                                                                                                       指定對應于 RIPrep 映射之 .sif 檔案的完整路徑和檔案名。 這個檔案通常稱為 Riprep，在包含 RIPrep 映射的資料夾的 \Templates 子資料夾中找到。                                                                                                                                                                                                       |
|        /DestinationImage        | 使用下列選項指定目的地映射的設定。</br>-/FilePath：\<檔案路徑和名稱 >-設定新檔案的完整檔案路徑。 例如： **C:\Temp\convert.wim**</br>-[/Name：\<名稱 >]-設定影像的顯示名稱。 如果未指定顯示名稱，則會使用來源影像的顯示名稱。</br>-[/Description： \<Description >]-設定影像的描述。</br>-[/InPlace]-指定應該在原始 RIPrep 映射上進行轉換，而不是在原始映射的複本上進行，這是預設行為。</br>-[/Overwrite： {Yes |

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要將指定的 RIPrep 映射轉換為 RIPREP .wim，請輸入：
```
WDSUTIL /Convert-RiPrepImage /FilePath:R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif /DestinationImage
/FilePath:C:\Temp\RIPREP.wim
```
若要將指定的 RIPrep 映射轉換為具有指定之名稱和描述的 RIPREP .wim，並以新的檔案覆寫它（如果檔案已經存在），請輸入：
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif
/DestinationImage /FilePath:\\Server\Share\RIPREP.wim
/Name:WindowsXP image /Description:Converted RIPREP image of WindowsXP
/Overwrite:Append
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)