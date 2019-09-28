---
title: Md
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 965a5c506535a2c52d6cc7b3557c6104182c12a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373695"
---
# <a name="md"></a>Md



建立目錄或子目錄。

> [!NOTE]
> 此命令與**mkdir**命令相同。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|@no__t 0Drive >：|指定您要在其上建立新目錄的磁片磁碟機。|
|\<Path >|必要。 指定新目錄的名稱和位置。 任何單一路徑的最大長度都是由檔案系統所決定。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

預設會啟用的命令延伸模組，可讓您使用單一**md**命令在指定的路徑中建立中繼目錄。

## <a name="BKMK_examples"></a>典型

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
cd \Taxes 
md Property
cd Property
md Current
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[Cmd](cmd.md)