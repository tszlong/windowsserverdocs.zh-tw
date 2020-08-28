---
title: select partition
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 816a235f7ba83320828a5dc72c9f2558c27b2ed8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027906"
---
# <a name="select-partition"></a>select partition

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的資料分割，並將焦點移至該分割區。 此命令也可以用來顯示目前在所選磁片中有焦點的磁碟分割。



## <a name="syntax"></a>語法

```
select partition=<n>
```

### <a name="parameters"></a>參數

|   參數    |                                                                                    描述                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 分區\=<n> | 要接收焦點的資料分割編號。 您可以使用 DiskPart 中的 **清單磁碟分割** 命令，來查看目前所選磁片上所有磁碟分割的數目。 |

## <a name="remarks"></a>備註

-   選取磁碟分割之前，您必須先使用 [ **選取磁片** ] 命令來選取磁片。

-   如果未指定資料分割編號，此命令會顯示目前在所選磁片中具有焦點的磁碟分割。

-   如果選取的磁片區具有對應的磁碟分割，則會自動選取該磁碟分割。

-   如果選取磁碟分割與對應的磁片區，則會自動選取該磁片區。

## <a name="examples"></a>範例
若要將焦點移至資料分割3，請輸入：

```
select partitition=3
```

若要顯示目前在所選磁片中具有焦點的磁碟分割，請輸入：

```
select partition
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)




