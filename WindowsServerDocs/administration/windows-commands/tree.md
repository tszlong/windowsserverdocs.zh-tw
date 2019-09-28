---
title: tree
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 22875e63526dc3465021c9aa990f6cea388b81e4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385625"
---
# <a name="tree"></a>tree



以圖形方式顯示磁片磁碟機中路徑或磁片的目錄結構。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
tree [<Drive>:][<Path>] [/f] [/a]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|@no__t 0Drive >：|指定包含您想要顯示目錄結構之磁片的磁片磁碟機。|
|\<Path >|指定您想要顯示目錄結構的目錄。|
|/f|顯示每個目錄中的檔案名。|
|/a|指定**樹狀結構**是使用文字字元，而不是圖形字元，以顯示連結子目錄的線條。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

**Tree**所顯示的結構取決於您在命令提示字元中指定的參數。 如果您未指定磁片磁碟機或路徑，**樹狀**結構會顯示以目前磁片磁碟機目前目錄開頭的樹狀結構。

## <a name="BKMK_examples"></a>典型

若要顯示目前磁片磁碟機中磁片上所有子目錄的名稱，請輸入：
```
tree \
```
若要顯示，一次一個畫面，磁片磁碟機 C 的所有目錄中的檔案，請輸入：
```
tree c:\ /f | more 
```
若要列印磁片磁碟機 C 上所有目錄的清單，請輸入：
```
tree c:\ /f  prn 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)