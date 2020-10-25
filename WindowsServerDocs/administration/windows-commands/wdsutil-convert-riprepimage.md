---
title: 轉換-riprepimage
description: Riprepimage 命令的參考文章，可將現有的遠端安裝準備 (RIPrep) 映射轉換成 Windows 映像 ( .wim) 格式。
ms.topic: reference
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4062aa747bd02956a498c16c9e8aa15e96d8170d
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524923"
---
# <a name="convert-riprepimage"></a>轉換-riprepimage

將現有的遠端安裝準備 (RIPrep) 映射轉換成 Windows 映像 ( .wim) 格式。

## <a name="syntax"></a>語法

```
wdsutil [Options] /Convert-RIPrepImage /FilePath:<Filepath and name> /DestinationImage /FilePath:<Filepath and name> [/Name:<Name>] [/Description:<Description>] [/InPlace] [/Overwrite:{Yes | No | Append}]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| FilePath`<Filepath and name>` | 指定對應至 RIPrep 映射之 .sif 檔案的完整檔案路徑和名稱。 此檔案通常稱為 Riprep，並且可在包含 RIPrep 映射之資料夾的 **\Templates** 子資料夾中找到。 |
| /DestinationImage | 指定目的地映射的設定。  使用下列選項：<ul><li>`/FilePath:<Filepath and name>` -設定新檔案的完整檔案路徑。 例如： **C:\Temp\convert.wim**</li><li>[ `/Name:<Name>` ]-設定影像的顯示名稱。 如果未指定顯示名稱，則會使用來源影像的顯示名稱。</li><li>[ `/Description:<Description>` ]-設定影像的描述。</li><li>[/InPlace]-指定應該在原始 RIPrep 映射上進行轉換，而不是在原始映射的複本上進行，這是預設行為。</li><li>[ `/Overwrite:{Yes | No | Append}` -設定此映射是否應該覆寫或附加任何現有的檔案。</li></ul> |

## <a name="examples"></a>範例

若要將指定的 RIPrep 映射轉換為 RIPREP .wim，請輸入：

```
wdsutil /Convert-RiPrepImage /FilePath:R:\RemoteInstall\Setup\English \Images\Win2k3.SP1\i386\Templates\riprep.sif /DestinationImage /FilePath:C:\Temp\RIPREP.wim
```

若要將指定的 RIPrep 映射轉換為具有指定之名稱和描述的 RIPREP，並以新檔案覆寫該檔案（如果檔案已存在），請輸入：

```
wdsutil /Verbose /Progress /Convert-RiPrepImage /FilePath:\\Server \RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif /DestinationImage /FilePath:\\Server\Share\RIPREP.wim /Name:WindowsXP image /Description:Converted RIPREP image of WindowsXP /Overwrite:Append
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
