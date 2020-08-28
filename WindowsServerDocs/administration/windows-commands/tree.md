---
title: tree
description: 樹狀目錄的參考文章，以圖形方式顯示路徑或磁片磁碟機中磁片的目錄結構。
ms.topic: reference
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c164fe8999313ffd40ec12b29c7ad8cf2bd7c7a5
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026886"
---
# <a name="tree"></a>tree

以圖形方式顯示磁片磁碟機路徑或磁片的目錄結構。



## <a name="syntax"></a>語法

```
tree [<Drive>:][<Path>] [/f] [/a]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive>:|指定包含您要顯示其目錄結構之磁片的磁片磁碟機。|
|\<Path>|指定您想要顯示目錄結構的目錄。|
|/f|顯示每個目錄中檔案的名稱。|
|/a|指定 **樹狀結構** 是使用文字字元而非圖形字元來顯示連結子目錄的行。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

**樹狀**結構所顯示的結構需視您在命令提示字元中指定的參數而定。 如果您未指定磁片磁碟機或路徑， **樹狀** 結構會顯示以目前磁片磁碟機的目前的目錄開頭的樹狀結構。

## <a name="examples"></a>範例

若要顯示目前磁片磁碟機中磁片上所有子目錄的名稱，請輸入：
```
tree \
```
若要一次顯示一個畫面、磁片磁碟機 C 上所有目錄中的檔案，請輸入：
```
tree c:\ /f | more
```
若要列印磁片磁碟機 C 上所有目錄的清單，請輸入：
```
tree c:\ /f  prn
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)