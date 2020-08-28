---
title: convert mbr
description: 轉換 mbr 命令的參考文章，此命令會將空的基本磁碟與 GUID 磁碟分割表格 (GPT) 磁碟分割樣式轉換成具有主開機記錄 (MBR) 磁碟分割樣式的基本磁碟。
ms.topic: reference
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a07ca0e6c3d07dadf416a04ac1c5c4adbeb5bfe
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034166"
---
# <a name="convert-mbr"></a>convert mbr

將具有 GUID 磁碟分割表格 (GPT) 磁碟分割樣式的空白基本磁碟轉換為具有主開機記錄 (MBR) 磁碟分割樣式的基本磁碟。 必須選取基本磁碟，此操作才會成功。 使用 [ [選取磁片] 命令](select-disk.md) 選取基本磁碟，並將焦點移至該磁片。

> [!IMPORTANT]
> 磁碟必須是空的，才能轉換成基本磁蹀。 在轉換磁碟之前，先備份您的資料，然後刪除全部磁碟分割或磁碟區。

> [!NOTE]
> 如需有關如何使用此命令的指示，請參閱 [將 GUID 磁碟分割表格磁片變更為主開機記錄磁片](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc725797(v=ws.11))。

## <a name="syntax"></a>語法

```
convert mbr [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要將基本光碟從 GPT 磁碟分割樣式轉換成 MBR 磁碟分區樣式，請輸入>：

```
convert mbr
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [轉換命令](convert.md)
