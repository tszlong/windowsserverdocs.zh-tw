---
title: Md
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1396038410ecc5db5a124a1768038c4f8c8bea8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820839"
---
# <a name="md"></a>Md



建立目錄或子目錄。

> [!NOTE]
> 此命令等同於**mkdir**命令。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<磁碟機 >:|指定您要建立新目錄的磁碟機。|
|\<Path>|必要。 指定的名稱和新的目錄位置。 任何單一路徑的最大長度取決於檔案系統。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

命令延伸模組，依預設會啟用，可讓您使用單一**md**命令來建立指定之路徑中的中繼目錄。

## <a name="BKMK_examples"></a>範例

若要建立名為 Directory1，目前的目錄內的目錄，請輸入：
```
md Directory1
```
若要建立命令延伸模組啟用目錄樹狀結構 Taxes\Property\Current 根目錄的目錄中，輸入：
```
md \Taxes\Property\Current
```
若要建立的目錄樹狀結構 Taxes\Property\Current，如同先前的範例，但停用的命令擴充功能與在根目錄中，輸入下列命令序列：
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