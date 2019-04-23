---
title: popd
description: 了解如何將目錄變更到最近儲存 pushd 命令的目錄。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6da6dc9d1fc2d8965f8a081831cb1150375209a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827459"
---
# <a name="popd"></a>popd

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將目前目錄變更至最近所儲存的目錄**pushd**命令。
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
-   每次您使用**pushd**命令，供您使用儲存單一目錄。 不過，您可以儲存多個目錄，使用**pushd**命令多次。
    目錄會依序儲存在虛擬的堆疊。 如果您使用**pushd**命令之後，您可以在其中使用命令的目錄會放在堆疊的底部。 如果您一次使用此命令，第二個目錄會放在第一個之上。 每次您使用的程序會重複**pushd**命令。
    您可以使用**popd**命令來將目前的目錄變更為最近儲存的目錄**pushd**命令。 如果您使用**popd**命令中，從堆疊中移除的堆疊頂端的目錄，並在目前的目錄變更至該目錄。 如果您使用**popd**同樣地，命令會移除在堆疊上的下一步 目錄。
-   命令延伸模組啟用時， **popd**命令會移除建立的任何磁碟機代號 assignations **pushd**。

## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**pushd**命令並**popd**命令批次程式，從批次程式已執行的並將它變更回變更目前的目錄中：

```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

## <a name="additional-references"></a>其他參考資料
-   [pushd](pushd.md)
-   [命令列語法關鍵](command-line-syntax-key.md)

