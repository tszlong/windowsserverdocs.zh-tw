---
title: attributes disk
description: Attribute disk 命令的參考文章，可顯示、設定或清除磁片的屬性。
ms.topic: reference
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c86d499f179453f75e05ae3a871de013e389341a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633305"
---
# <a name="attributes-disk"></a>attributes disk

顯示、設定或清除磁片的屬性。 當此命令用來顯示磁片的目前屬性時，startup disk 屬性代表用來啟動電腦的磁片。 若為動態鏡像，它會顯示包含開機磁碟區之開機 plex 的磁片。

> [!IMPORTANT]
> 必須選取磁片， **屬性磁片** 命令才能成功。 使用 [ **選取磁片** ] 命令來選取磁片，並將焦點移至該磁片。

## <a name="syntax"></a>語法

```
attributes disk [{set | clear}] [readonly] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| set | 以焦點設定磁片的指定屬性。 |
| clear | 清除具有焦點之磁片的指定屬性。 |
| readonly | 指定磁片是唯讀的。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要查看所選磁片的屬性，請輸入：

```
attributes disk
```

若要將選取的磁片設定為唯讀，請輸入：

```
attributes disk set readonly
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取磁片命令](select-disk.md)
