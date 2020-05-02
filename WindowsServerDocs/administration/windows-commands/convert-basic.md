---
title: convert basic
description: Convert basic 命令的參考主題，可將空的動態磁碟轉換為基本磁碟。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e44ecc9f5d18bbe426c63f8854e7c3347f418bb2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720786"
---
# <a name="convert-basic"></a>convert basic

將空的動態磁碟轉換為基本磁碟。 必須選取動態磁碟，此操作才會成功。 使用 [[選取磁片] 命令](select-disk.md)來選取動態磁碟，並將焦點移至其上。

> [!IMPORTANT]
> 磁碟必須是空的，才能轉換成基本磁蹀。 在轉換磁碟之前，先備份您的資料，然後刪除全部磁碟分割或磁碟區。

> [!NOTE]
> 如需有關如何使用此命令的指示，請參閱[將動態磁碟變更回基本磁碟](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755238(v=ws.11))）。

## <a name="syntax"></a>語法

```
convert basic [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="examples"></a>範例

若要將選取的動態磁碟轉換為基本，請輸入：

```
convert basic
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [轉換命令](convert.md)
