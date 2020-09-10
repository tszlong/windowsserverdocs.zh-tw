---
title: remove
description: 移除命令的參考文章，此命令會從磁片區移除磁碟機號或掛接點。
ms.topic: reference
ms.assetid: b0886140-da8b-4231-8cb2-f280874d99c0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ba7d625c5908af4a209266293495e6d472cb730b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641037"
---
# <a name="remove"></a>remove

從帶有焦點的磁碟區移除磁碟機代號或掛接點。 如果使用 all 參數，則會移除全部目前磁碟機代號及掛接點。 如果未指定磁碟機號或掛接點，則 DiskPart 會移除第一個遇到的磁碟機號或掛接點。

[移除] 命令也可以用來變更與卸載式磁片磁碟機相關聯的磁碟機號。 您無法在系統、開機或分頁磁片區上移除磁碟機號。 此外，您無法移除 OEM 磁碟分割的磁碟機號、具有無法辨識 GUID 的 GPT 磁碟分割，或任何特殊、非資料的 GPT 磁碟分割，例如 EFI 系統磁碟分割。

> [!NOTE]
> 必須選取磁片區，才能讓 [ **移除** ] 命令成功。 使用 [ [選取](select-volume.md) 磁片區] 命令來選取磁片，並將焦點移至該磁片。

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
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

### <a name="examples"></a>範例

移除 d:\磁片磁碟機，輸入：

```
remove letter=d
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
