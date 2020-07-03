---
title: select volume
description: '* * * * 的參考文章'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 847cf4865217dd4165f6973f1019dffa22ec85ec
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935939"
---
# <a name="select-volume"></a>select volume

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的磁片區，並將焦點移至其上。 此命令也可以用來顯示目前在所選磁片中具有焦點的磁片區。



## <a name="syntax"></a>語法

```
select volume={<n>|<d>}
```

### <a name="parameters"></a>參數

| 參數 |                                                                               說明                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <n>    | 要接收焦點的磁片區編號。 您可以使用 DiskPart 中的 [**列出**磁片區] 命令，來查看目前選取之磁片上的所有磁片區編號。 |
|    <d>    |                                                 要接收焦點之磁片區的磁碟機號或掛接點路徑。                                                 |

## <a name="remarks"></a>備註

-   如果未指定磁片區，此命令會顯示目前在所選磁片中具有焦點的磁片區。

-   在基本磁碟上，選取磁片區也會將焦點提供給對應的分割區。

-   如果選取了具有對應磁碟分割的磁片區，則會自動選取該磁碟分割。

-   如果選取的資料分割具有對應的磁片區，則會自動選取該磁片區。

## <a name="examples"></a>範例
若要將焦點移到第2卷，請輸入：

```
select volume=2
```

若要將焦點移至 C 磁片磁碟機，請輸入：

```
select volume=c
```

若要將焦點移至名為 mountpath 之資料夾上掛接的磁片區，請輸入：

```
select volume=c:\mountpath
```

若要顯示目前在所選磁片中具有焦點的音量，請輸入：

```
select volume
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)




