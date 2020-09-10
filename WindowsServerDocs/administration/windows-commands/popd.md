---
title: popd
description: Pnputil 命令的參考文章，此命令會將目前的目錄變更為最新的 pushd 命令所儲存的目錄。
ms.topic: reference
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 4884e294f878a55125a035d7113ce857e1964634
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633652"
---
# <a name="popd"></a>popd

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**Popd**命令會將目前的目錄變更為由**pushd**命令最近儲存的目錄。

每次使用 **pushd** 命令時，即會儲存單一目錄以供您使用。 不過，您可以使用 **pushd** 命令多次儲存多個目錄。 目錄會依序儲存在虛擬堆疊中，因此如果您使用了 **pushd** 命令一次，則會在堆疊底部放置您使用命令的目錄。 如果您再次使用命令，則會將第二個目錄放在第一個目錄的最上層。 每次您使用 **pushd** 命令時，都會重複此程式。

如果您使用 **popd** 命令，則會移除堆疊頂端的目錄，並將目前的目錄變更為該目錄。 如果您再次使用 **popd** 命令，則會移除堆疊下的下一個目錄。 如果命令延伸模組已啟用，則 **popd** 命令會移除由 **pushd** 命令所建立的任何磁碟機號 assignations。

## <a name="syntax"></a>語法

```
popd
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要變更執行 batch 程式的目錄，然後將它變更回來，請輸入：

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

- [命令列語法關鍵](command-line-syntax-key.md)

- [pushd](pushd.md)
