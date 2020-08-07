---
title: popd
description: Pnputil 命令的參考文章，它會將目前的目錄變更為由 pushd 命令最近儲存的目錄。
ms.topic: article
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: d78566cf53d7618d0fb5b7fc57ca02b8ee82484c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884914"
---
# <a name="popd"></a>popd

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**Popd**命令會將目前的目錄變更為由**pushd**命令最近儲存的目錄。

每次使用**pushd**命令時，即會儲存單一目錄以供您使用。 不過，您可以多次使用**pushd**命令來儲存多個目錄。 目錄會依序儲存在虛擬堆疊中，因此，如果您使用**pushd**命令一次，則使用命令的目錄會放在堆疊的底部。 如果您再次使用命令，第二個目錄就會放在第一個目錄的頂端。 每次使用**pushd**命令時，都會重複此程式。

如果您使用**popd**命令，則會移除堆疊頂端的目錄，並將目前的目錄變更為該目錄。 如果您再次使用**popd**命令，則會移除堆疊上的下一個目錄。 如果已啟用命令延伸模組，則**popd**命令會移除由**pushd**命令所建立的任何磁碟機號 assignations。

## <a name="syntax"></a>語法

```
popd
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要從執行批次程式的來源中變更目前的目錄，然後將它變更回來，請輸入：

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
