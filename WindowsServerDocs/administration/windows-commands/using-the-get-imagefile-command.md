---
title: 使用 get-ImageFile 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c8136585e04caca02ab16c7b4ca11a825cf400d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392140"
---
# <a name="using-the-get-imagefile-command"></a>使用 get-ImageFile 命令



抓取 Windows 映像（.wim）檔案中所含映射的相關資訊。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/ImageFile： \<WIM 檔案路徑 >|指定 .wim 檔案的完整路徑和檔案名。|
|[/Detailed]|傳回每個影像中的所有影像中繼資料。 如果未使用此選項，則預設行為是只傳回映射名稱、描述和檔案名。|

## <a name="BKMK_examples"></a>典型

若要查看影像的相關資訊，請輸入：
```
WDSUTIL /Get-ImageFile /ImageFile:"C:\temp\install.wim"
```
若要查看詳細資訊，請輸入：
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:"\\Server\Share\My Folder \install.wim" /Detailed
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)