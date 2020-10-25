---
title: 取得-ImageFile
description: Get-ImageFile 的參考文章，可抓取 Windows 映像中所含映射的相關資訊 ( .wim) 檔。
ms.topic: reference
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 41183f92d75736afc32dfbbee9d31871b3ef24f9
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524333"
---
# <a name="get-imagefile"></a>取得-ImageFile

抓取 Windows 映像中包含的映射 ( .wim) 檔的相關資訊。

## <a name="syntax"></a>語法

```
wdsutil [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|映射檔\<WIM file path>|指定 .wim 檔案的完整路徑和檔案名。|
|[/Detailed]|傳回每個影像的所有影像中繼資料。 如果未使用此選項，預設行為是只傳回映射名稱、描述和檔案名。|

## <a name="examples"></a>範例

若要查看影像的相關資訊，請輸入：
```
wdsutil /Get-ImageFile /ImageFile:C:\temp\install.wim
```
若要查看詳細資訊，請輸入：
```
wdsutil /Verbose /Get-ImageFile /ImageFile:\\Server\Share\My Folder \install.wim /Detailed
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)