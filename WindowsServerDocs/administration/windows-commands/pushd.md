---
title: pushd
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 548f39921c1f6aa3837e6443e396922396eb84f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887459"
---
# <a name="pushd"></a>pushd



儲存目前的目錄，以供**popd**命令，然後再對指定的目錄。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
pushd [<Path>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Path>|指定要讓目前的目錄的目錄。 此命令還支援相對路徑。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   每次您使用**pushd**命令，供您使用儲存單一目錄。 不過，您可以儲存多個目錄，使用**pushd**命令多次。

    目錄會依序儲存在虛擬的堆疊。 如果您使用**pushd**命令之後，您可以在其中使用命令的目錄會放在堆疊的底部。 如果您一次使用此命令，第二個目錄會放在第一個之上。 每次您使用的程序會重複**pushd**命令。

    您可以使用**popd**命令來將目前的目錄變更為最近儲存的目錄**pushd**命令。 如果您使用**popd**命令中，從堆疊中移除的堆疊頂端的目錄，並在目前的目錄變更至該目錄。 如果您使用**popd**同樣地，命令會移除在堆疊上的下一步 目錄。
-   如果已啟用命令延伸模組， **pushd**命令可接受的網路路徑或本機磁碟機代號和路徑。
-   如果您指定網路路徑**pushd**命令暫時指派最高的未使用的磁碟機代號 （從 z： 開始）指定的網路資源。 命令接著會變更目前磁碟機和目錄為新指派的磁碟機上指定的目錄。 如果您使用**popd**命令搭配已啟用，擴充命令**popd**命令會移除所建立的磁碟機代號指派**pushd**。

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

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[Popd](popd.md)