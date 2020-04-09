---
title: convert basic
description: 適用于 convert basic 的 Windows 命令主題，可將空的動態磁碟轉換為基本磁碟。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e0f6f5f04373042956d83bc9136c884c268e591
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847311"
---
# <a name="convert-basic"></a>convert basic

將空的動態磁碟轉換為基本磁碟。

如需有關如何使用此命令的指示，請參閱[將動態磁碟變更回基本磁碟](https://go.microsoft.com/fwlink/?LinkId=207048)（ https://go.microsoft.com/fwlink/?LinkId=207048)。

## <a name="syntax"></a>語法

```
convert basic [noerr]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

> [!IMPORTANT]
> 磁碟必須是空的，才能轉換成基本磁蹀。 在轉換磁碟之前，先備份您的資料，然後刪除全部磁碟分割或磁碟區。

-   必須選取動態磁碟，此操作才會成功。 使用 [**選取磁片**] 命令來選取動態磁碟，並將焦點移至其上。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要將選取的動態磁碟轉換為基本，請輸入：
```
convert basic
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

