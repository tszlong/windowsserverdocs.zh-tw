---
title: convert gpt
description: Convert gpt 命令的參考文章，此命令會將具有主開機記錄的空白基本磁碟轉換成具有 GUID 磁碟分割)  (表格的基本磁碟 (MBR) 磁碟分割樣式。
ms.topic: reference
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 04b1bacee89e7553aa4ed37337c4667bc93d5da3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025882"
---
# <a name="convert-gpt"></a>convert gpt

將具有主開機記錄 (MBR) 磁碟分割樣式的空基本磁碟轉換成具有 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的基本磁碟。 必須選取基本的 MBR 磁碟，此操作才會成功。 使用 [ [選取磁片] 命令](select-disk.md) 選取基本磁碟，並將焦點移至該磁片。

> [!IMPORTANT]
> 磁碟必須是空的，才能轉換成基本磁蹀。 在轉換磁碟之前，先備份您的資料，然後刪除全部磁碟分割或磁碟區。 轉換至 GPT 所需的磁片大小下限為 128 mb。

> [!NOTE]
> 如需有關如何使用此命令的指示，請參閱 [將主開機記錄磁片變更為 GUID 磁碟分割表格磁片](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc725671(v=ws.11))。

## <a name="syntax"></a>語法

```
convert gpt [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要將基本光碟從 MBR 磁碟分區樣式轉換成 GPT 磁碟分割樣式，請輸入：

```
convert gpt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [轉換命令](convert.md)
