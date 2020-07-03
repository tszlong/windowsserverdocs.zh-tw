---
title: convert dynamic
description: 轉換動態命令的參考文章，其會將基本磁碟轉換成動態磁碟。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3474da8c1f4a3e58d9e77c36abe4390ec2f5b731
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928978"
---
# <a name="convert-dynamic"></a>convert dynamic

將基本磁碟轉換為動態磁碟。 必須選取基本磁碟，此操作才能成功。 使用 [[選取磁片] 命令](select-disk.md)來選取基本磁碟，並將焦點轉移到其上。

> [!NOTE]
> 如需有關如何使用此命令的指示，請參閱[將動態磁碟變更回基本磁碟](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755238(v=ws.11))）。

## <a name="syntax"></a>語法

```
convert dynamic [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

#### <a name="remarks"></a>備註

- 基本磁碟上的任何現有磁碟分割都會變成簡單磁片區。

## <a name="examples"></a>範例

若要將基本磁碟轉換成動態磁碟，請輸入：

```
convert dynamic
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [轉換命令](convert.md)
