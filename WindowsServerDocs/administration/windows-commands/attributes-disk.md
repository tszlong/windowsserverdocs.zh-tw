---
title: attributes disk
description: 屬性磁片命令的參考文章，它會顯示、設定或清除磁片的屬性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 02ad39b84afb2487b388d046d6409a682b58615b
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923894"
---
# <a name="attributes-disk"></a>attributes disk

顯示、設定或清除磁片的屬性。 當此命令用來顯示磁片的目前屬性時，啟動磁片屬性代表用來啟動電腦的磁片。 若為動態鏡像，它會顯示包含開機磁碟區之開機 plex 的磁片。

> [!IMPORTANT]
> 必須選取磁片，**屬性 disk**命令才會成功。 使用 [**選取磁片**] 命令來選取磁片，並將焦點移至它。

## <a name="syntax"></a>語法

```
attributes disk [{set | clear}] [readonly] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| set | 設定具有焦點之磁片的指定屬性。 |
| clear | 清除具有焦點之磁片的指定屬性。 |
| readonly | 指定磁片是唯讀的。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

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
