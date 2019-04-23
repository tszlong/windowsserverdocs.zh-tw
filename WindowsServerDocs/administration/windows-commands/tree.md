---
title: tree
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de22de1c9d62ba79c1aa68248109cca88009703a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872629"
---
# <a name="tree"></a>tree



以圖形方式顯示目錄結構的路徑或磁碟的磁碟機中。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
tree [<Drive>:][<Path>] [/f] [/a]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<磁碟機 >:|指定包含您要顯示的目錄結構之的磁碟的磁碟機。|
|\<Path>|指定您要顯示的目錄結構的目錄。|
|/f|每個目錄中顯示的檔案名稱。|
|/a|指定**樹狀結構**是使用文字字元而非圖形字元，以顯示連結子目錄的線條。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

所顯示的結構**樹狀結構**取決於您在命令提示字元中指定的參數。 如果您未指定磁碟機或路徑，**樹狀結構**會顯示目前的磁碟機的目前目錄樹狀結構開頭。

## <a name="BKMK_examples"></a>範例

若要顯示在您目前的磁碟機的磁碟上的所有子目錄的名稱，請輸入：
```
tree \
```
若要顯示，一次一個畫面中的所有目錄的檔案，磁碟機 C，輸入：
```
tree c:\ /f | more 
```
若要列印 C 磁碟機上的所有目錄的清單，請輸入：
```
tree c:\ /f  prn 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)