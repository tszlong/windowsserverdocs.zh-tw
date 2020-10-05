---
title: 取得-ImageFile
description: Get-ImageFile 的參考文章，可抓取 Windows 映像中所含映射的相關資訊 ( .wim) 檔。
ms.topic: reference
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c358c490424406e6eb8d709c879f043ce017164d
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729893"
---
# <a name="get-imagefile"></a>取得-ImageFile

抓取 Windows 映像中包含的映射 ( .wim) 檔的相關資訊。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|映射檔\<WIM file path>|指定 .wim 檔案的完整路徑和檔案名。|
|[/Detailed]|傳回每個影像的所有影像中繼資料。 如果未使用此選項，預設行為是只傳回映射名稱、描述和檔案名。|

## <a name="examples"></a>範例

若要查看影像的相關資訊，請輸入：
```
WDSUTIL /Get-ImageFile /ImageFile:C:\temp\install.wim
```
若要查看詳細資訊，請輸入：
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:\\Server\Share\My Folder \install.wim /Detailed
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)