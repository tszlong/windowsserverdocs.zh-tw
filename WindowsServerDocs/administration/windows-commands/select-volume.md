---
title: select volume
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d2737a25eb9095b70fd6939a4f38b751868323f3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024982"
---
# <a name="select-volume"></a>select volume

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的磁片區，並將焦點移至該磁片區。 此命令也可以用來顯示目前在所選磁片中有焦點的磁片區。



## <a name="syntax"></a>語法

```
select volume={<n>|<d>}
```

### <a name="parameters"></a>參數

| 參數 |                                                                               描述                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <n>    | 要接收焦點的磁片區數目。 您可以使用 DiskPart 中的 **list volume** 命令，來查看目前所選磁片上所有磁片區的數目。 |
|    <d>    |                                                 要接收焦點之磁片區的磁碟機號或掛接點路徑。                                                 |

## <a name="remarks"></a>備註

-   如果未指定磁片區，此命令會顯示目前在所選磁片中具有焦點的磁片區。

-   在基本磁碟上，選取磁片區也會將焦點提供給對應的磁碟分割。

-   如果選取的磁片區具有對應的磁碟分割，則會自動選取該磁碟分割。

-   如果選取磁碟分割與對應的磁片區，則會自動選取該磁片區。

## <a name="examples"></a>範例
若要將焦點移至磁片區2，請輸入：

```
select volume=2
```

若要將焦點移到 C 磁片磁碟機，請輸入：

```
select volume=c
```

若要將焦點移至名為 mountpath 的資料夾上所裝載的磁片區，請輸入：

```
select volume=c:\mountpath
```

若要顯示目前在所選磁片中具有焦點的磁片區，請輸入：

```
select volume
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)




