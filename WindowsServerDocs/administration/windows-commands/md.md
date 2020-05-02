---
title: Md
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a605571fb74af99d0f365a100dd33fd4db0d3f22
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723999"
---
# <a name="md"></a>Md



建立目錄或子目錄。

> [!NOTE]
> 此命令與**mkdir**命令相同。



## <a name="syntax"></a>語法

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<磁片磁碟機>：|指定您要在其上建立新目錄的磁片磁碟機。|
|\<路徑>|必要。 指定新目錄的名稱和位置。 任何單一路徑的最大長度都是由檔案系統所決定。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

預設會啟用的命令延伸模組，可讓您使用單一**md**命令在指定的路徑中建立中繼目錄。

## <a name="examples"></a>範例

若要在目前目錄中建立名為 Directory1 的目錄，請輸入：
```
md Directory1
```
若要建立根目錄內的目錄樹狀結構 Taxes\Property\Current，並啟用命令延伸模組，請輸入：
```
md \Taxes\Property\Current
```
如先前範例所示，若要在根目錄中建立目錄樹狀結構 Taxes\Property\Current，但已停用命令延伸模組，請輸入下列順序的命令：
```
md \Taxes
md \Taxes\Property
md \Taxes\Property\Current
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

[Cmd](cmd.md)