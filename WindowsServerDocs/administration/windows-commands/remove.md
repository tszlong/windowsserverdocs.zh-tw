---
title: remove
description: '[移除] 命令的參考文章，會從磁片區移除磁碟機號或掛接點。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b0886140-da8b-4231-8cb2-f280874d99c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fba062945effd49efa9987d2c7d23fc90552fd4
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409679"
---
# <a name="remove"></a>remove

從帶有焦點的磁碟區移除磁碟機代號或掛接點。 如果使用 all 參數，則會移除全部目前磁碟機代號及掛接點。 如果未指定磁碟機號或掛接點，則 DiskPart 會移除第一個遇到的磁碟機號或掛接點。

[移除] 命令也可以用來變更與卸載式磁片磁碟機相關聯的磁碟機號。 您無法移除系統、開機或分頁磁片區上的磁碟機號。 此外，您無法移除 OEM 磁碟分割的磁碟機號、任何具有無法辨識 GUID 的 GPT 磁碟分割，或任何特殊、非資料、GPT 磁碟分割（如 EFI 系統磁碟分割）。

> [!NOTE]
> 必須選取磁片區，[**移除**] 命令才會成功。 使用 [[選取](select-volume.md)磁片區] 命令來選取磁片，並將焦點移至它。

## <a name="syntax"></a>語法

```
remove [{letter=<drive> | mount=<path> [all]}] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 字母 =`<drive>` | 要移除的磁碟機號。 |
| 掛接 =`<path>` | 要移除的掛接點路徑。 |
| all | 移除全部目前磁碟機代號及掛接點。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

### <a name="examples"></a>範例

移除 d:\磁片磁碟機，請輸入：

```
remove letter=d
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
