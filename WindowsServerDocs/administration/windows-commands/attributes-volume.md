---
title: attributes volume
description: '[屬性] [volume] 命令的參考文章，可顯示、設定或清除磁片區的屬性。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aae7cc7fe26fac5ef03e40610eb46389eb274c94
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923875"
---
# <a name="attributes-volume"></a>attributes volume

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示、設定或清除磁片區的屬性。

## <a name="syntax"></a>語法

```
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| ------- | -------- |
| set | 設定具有焦點之磁片區的指定屬性。 |
| clear | 清除具有焦點之磁片區的指定屬性。 |
| readonly | 指定磁碟區為唯讀。 |
| 隱藏 | 指定磁碟區為隱藏。 |
| nodefaultdriveletter | 指定磁碟區根據預設不接受磁碟機代號。 |
| shadowcopy | 指定磁碟區為陰影複製磁碟區。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

### <a name="remarks"></a>備註

- 在基本的主開機記錄（MBR）磁片上， **hidden**、 **readonly**和**nodefaultdriveletter**參數適用于磁片上的所有磁片區。

- 在基本 GUID 磁碟分割表格（GPT）磁片上，以及在動態 MBR 和 GPT 磁片上， **hidden**、 **readonly**和**nodefaultdriveletter**參數只適用于選取的磁片區。

- 必須選取磁片區，**屬性 volume**命令才會成功。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。

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
