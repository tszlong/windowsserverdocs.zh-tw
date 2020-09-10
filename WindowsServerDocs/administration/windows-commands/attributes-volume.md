---
title: attributes volume
description: 屬性磁片區命令的參考文章，可顯示、設定或清除磁片區的屬性。
ms.topic: reference
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dd651732dbf537da31ae5f5343c687868ffb2741
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633284"
---
# <a name="attributes-volume"></a>attributes volume

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示、設定或清除磁片區的屬性。

## <a name="syntax"></a>語法

```
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| set | 設定具有焦點之磁片區的指定屬性。 |
| clear | 清除具有焦點之磁片區的指定屬性。 |
| readonly | 指定磁碟區為唯讀。 |
| 隱藏 | 指定磁碟區為隱藏。 |
| nodefaultdriveletter | 指定磁碟區根據預設不接受磁碟機代號。 |
| shadowcopy | 指定磁碟區為陰影複製磁碟區。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

### <a name="remarks"></a>備註

- 在基本主機開機記錄 (MBR) 磁片上， **hidden**、 **readonly**和 **nodefaultdriveletter** 參數適用于磁片上的所有磁片區。

- 在基本 GUID 磁碟分割表格上 (GPT) 磁片，而在動態 MBR 和 GPT 磁片上， **hidden**、 **readonly**和 **nodefaultdriveletter** 參數只適用于選取的磁片區。

- 必須選取磁片區， **屬性磁片** 區命令才能成功。 使用 [ **選取磁片** 區] 命令來選取磁片區，並將焦點移至該磁片區。

## <a name="examples"></a>範例

若要在選取的磁片區上顯示目前的屬性，請輸入：

```
attributes volume
```

若要將選取的磁片區設定為隱藏和唯讀，請輸入：

```
attributes volume set hidden readonly
```

若要移除所選磁片區上的隱藏和唯讀屬性，請輸入：

```
attributes volume clear hidden readonly
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取磁片區命令](select-volume.md)
