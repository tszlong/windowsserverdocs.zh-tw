---
title: get-ImageFile
description: Get-ImageFile 的參考主題，它會抓取 Windows 映像（.wim）檔案中所含映射的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f60be17f13e1436a0e895991c72d5ccb7130782
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719908"
---
# <a name="get-imagefile"></a>get-ImageFile

抓取 Windows 映像（.wim）檔案中所含映射的相關資訊。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/ImageFile：\<WIM 檔案路徑>|指定 .wim 檔案的完整路徑和檔案名。|
|[/Detailed]|傳回每個影像中的所有影像中繼資料。 如果未使用此選項，則預設行為是只傳回映射名稱、描述和檔案名。|

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