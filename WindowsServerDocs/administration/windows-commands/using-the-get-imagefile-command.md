---
title: 使用 get ImageFile 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6bbe5ece95d1f9821a27b96e56bc34576a0f5f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827619"
---
# <a name="using-the-get-imagefile-command"></a>使用 get ImageFile 命令



擷取 Windows 映像 (.wim) 檔案中包含的映像的相關資訊。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/ ImageFile:\<WIM 檔案路徑 >|指定之.wim 檔案的完整路徑和檔案名稱。|
|[/ 詳細]|傳回從每個映像的映像的所有中繼資料。 如果未使用此選項，預設行為是傳回僅映像名稱、 描述和檔案名稱。|

## <a name="BKMK_examples"></a>範例

若要檢視映像的相關資訊，請輸入：
```
WDSUTIL /Get-ImageFile /ImageFile:"C:\temp\install.wim"
```
若要檢視的詳細的資訊，請輸入：
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:"\\Server\Share\My Folder \install.wim" /Detailed
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)