---
title: get-ImageFile
description: Get-ImageFile 的參考文章，它會抓取 Windows 映像（.wim）檔案中所含映射的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 127b5282b74020f002c7ccc55663fc2571584582
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932227"
---
# <a name="get-imagefile"></a>get-ImageFile

抓取 Windows 映像（.wim）檔案中所含映射的相關資訊。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|映射檔\<WIM file path>|指定 .wim 檔案的完整路徑和檔案名。|
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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)