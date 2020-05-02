---
title: 樹狀
description: 樹狀結構的參考主題，會以圖形方式顯示路徑的目錄結構，或磁片磁碟機中的磁片。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94b0429dadc3965c7e41ad5aa881fc902988ec9b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721278"
---
# <a name="tree"></a>樹狀

以圖形方式顯示磁片磁碟機中路徑或磁片的目錄結構。



## <a name="syntax"></a>語法

```
tree [<Drive>:][<Path>] [/f] [/a]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<磁片磁碟機>：|指定包含您想要顯示目錄結構之磁片的磁片磁碟機。|
|\<路徑>|指定您想要顯示目錄結構的目錄。|
|/f|顯示每個目錄中的檔案名。|
|/a|指定**樹狀結構**是使用文字字元，而不是圖形字元，以顯示連結子目錄的線條。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

**Tree**所顯示的結構取決於您在命令提示字元中指定的參數。 如果您未指定磁片磁碟機或路徑，**樹狀**結構會顯示以目前磁片磁碟機目前目錄開頭的樹狀結構。

## <a name="examples"></a>範例

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

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)