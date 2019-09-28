---
title: popd
description: 瞭解如何將目錄變更為由 pushd 命令最近儲存的目錄。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 8a9e0a301a5f8b46e1907a4f43c5ed9247b85f77
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372235"
---
# <a name="popd"></a>popd

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將目前的目錄變更為由**pushd**命令最近儲存的目錄。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法
```
popd
```

### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   每次使用**pushd**命令時，即會儲存單一目錄以供您使用。 不過，您可以多次使用**pushd**命令來儲存多個目錄。
    這些目錄會依序儲存在虛擬堆疊中。 如果您使用**pushd**命令一次，則使用命令的目錄會放在堆疊的底部。 如果您再次使用命令，第二個目錄就會放在第一個目錄的頂端。 每次使用**pushd**命令時，都會重複此程式。
    您可以使用**popd**命令，將目前的目錄變更為由**pushd**命令最近儲存的目錄。 如果您使用**popd**命令，堆疊頂端的目錄會從堆疊中移除，而目前的目錄則會變更為該目錄。 如果您再次使用**popd**命令，則會移除堆疊上的下一個目錄。
-   啟用命令延伸模組時， **popd**命令會移除由**pushd**所建立的任何磁碟機號 assignations。

## <a name="BKMK_examples"></a>典型
下列範例顯示如何在 batch 程式中使用**pushd**命令和**popd**命令，以變更執行 batch 程式的目錄，然後將它變更回來：

```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

## <a name="additional-references"></a>其他參考
-   [pushd](pushd.md)
-   [命令列語法關鍵](command-line-syntax-key.md)

