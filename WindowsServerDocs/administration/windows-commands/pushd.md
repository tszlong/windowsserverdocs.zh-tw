---
title: pushd
description: Pushd 命令的參考文章，它會儲存目前目錄以供 popd 命令使用，然後變更為指定的目錄。
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e885cab58e23af9da050822c49a655ca897ce819
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884585"
---
# <a name="pushd"></a>pushd

儲存目前目錄以供**popd**命令使用，然後變更為指定的目錄。

每次使用**pushd**命令時，即會儲存單一目錄以供您使用。 不過，您可以多次使用**pushd**命令來儲存多個目錄。 目錄會依序儲存在虛擬堆疊中，因此，如果您使用**pushd**命令一次，則使用命令的目錄會放在堆疊的底部。 如果您再次使用命令，第二個目錄就會放在第一個目錄的頂端。 每次使用**pushd**命令時，都會重複此程式。

如果您使用**popd**命令，則會移除堆疊頂端的目錄，並將目前的目錄變更為該目錄。 如果您再次使用**popd**命令，則會移除堆疊上的下一個目錄。 如果已啟用命令延伸模組，則**popd**命令會移除由**pushd**命令所建立的任何磁碟機號 assignations。

## <a name="syntax"></a>語法

```
pushd [<path>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<path>` | 指定要做為目前的目錄的目錄。 此命令支援相對路徑。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果已啟用命令延伸模組，則**pushd**命令會接受網路路徑或本機磁碟機號和路徑。

- 如果您指定網路路徑，則**pushd**命令會暫時將最高未使用的磁碟機號（從 Z： ) 開始指派給指定的網路資源） (。 然後，此命令會將目前的磁片磁碟機和目錄變更為新指派磁片磁碟機上的指定目錄。 如果您使用**popd**命令並啟用命令延伸模組，則**popd**命令會移除由**pushd**所建立的磁碟機號指派。

### <a name="examples"></a>範例

若要從執行批次程式的來源中變更目前的目錄，然後將它變更回來：

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

- [popd 命令](popd.md)
