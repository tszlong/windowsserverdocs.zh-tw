---
title: convert gpt
description: Convert gpt 命令的參考文章，其會將具有主開機記錄（MBR）磁碟分割樣式的空白基本磁碟轉換成具有 GUID 磁碟分割表格（GPT）磁碟分割樣式的基本磁碟。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8025e897a8ce7083b938e984f9a11b6c32ed312
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928956"
---
# <a name="convert-gpt"></a>convert gpt

將具有主開機記錄 (MBR) 磁碟分割樣式的空基本磁碟轉換成具有 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的基本磁碟。 必須選取基本的 MBR 磁碟，此操作才能成功。 使用 [[選取磁片] 命令](select-disk.md)來選取基本磁碟，並將焦點轉移到其上。

> [!IMPORTANT]
> 磁碟必須是空的，才能轉換成基本磁蹀。 在轉換磁碟之前，先備份您的資料，然後刪除全部磁碟分割或磁碟區。 轉換至 GPT 所需的最小磁片大小為 128 mb。

> [!NOTE]
> 如需有關如何使用此命令的指示，請參閱[將主開機記錄磁片變更為 GUID 磁碟分割表格磁片](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725671(v=ws.11))。

## <a name="syntax"></a>語法

```
convert gpt [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="examples"></a>範例

若要將基本光碟從 MBR 磁碟分區樣式轉換為 GPT 磁碟分割樣式，請輸入：

```
convert gpt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [轉換命令](convert.md)
