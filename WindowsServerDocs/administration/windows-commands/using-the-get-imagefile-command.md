---
title: get-ImageFile
description: Get-ImageFile 的參考文章，它會抓取 Windows 映像中所含映射 ( .wim) 檔案的相關資訊。
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10b86929366cc8734a5ff155200fb3078f0cd5d4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879373"
---
# <a name="get-imagefile"></a>get-ImageFile

抓取 Windows 映像中所含映射 ( .wim) 檔案的相關資訊。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

### <a name="parameters"></a>參數

|參數|描述|
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