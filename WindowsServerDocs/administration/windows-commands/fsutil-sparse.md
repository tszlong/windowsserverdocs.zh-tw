---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: Fsutil sparse
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: f9fb3cf46afb7e96c13fb623bc8f4fe67c1f3694
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376812"
---
# <a name="fsutil-sparse"></a>Fsutil sparse
>適用於：Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

管理稀疏檔案。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil sparse [queryflag] <FileName>
fsutil sparse [queryrange] <FileName>
fsutil sparse [setflag] <FileName>
fsutil sparse [setrange] <FileName> <BeginningOffset> <Length>
```

## <a name="parameters"></a>參數

|     參數     |                                                    描述                                                    |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
|     queryflag     |                                                  查詢稀疏。                                                  |
|    queryrange     |                        掃描檔案，並搜尋可能包含非零資料的範圍。                        |
|      setflag      |                                        將指定的檔案標示為 sparse。                                        |
|     setrange      |                                   以零填滿指定的檔案範圍。                                   |
|    <FileName>     | 指定檔案的完整路徑，包括檔案名和副檔名，例如 C:\documents\filename.txt。 |
| <BeginningOffset> |                              指定要標記為稀疏的檔案中的位移。                              |
|     <Length>      |                 指定要標記為稀疏之檔案中的區域長度（以位元組為單位）。                 |

## <a name="remarks"></a>備註

-   「稀疏檔案」（sparse file）是一個檔案，其中包含一或多個未配置資料的區域。 程式會將這些未配置的區域視為包含值為零的位元組，但不會使用磁碟空間來表示這些零。 會配置所有有意義或非零的資料，而不會配置所有的 nonmeaningful 資料（由零組成的大型字串資料）。 當讀取稀疏檔案時，配置的資料會以預存的形式傳回，而且根據 C2 安全性需求規格，預設會將未配置的資料傳回為零。 稀疏檔案支援可讓資料從檔案中的任何位置解除配置。

-   在稀疏檔案中，大型的零範圍可能不需要磁片配置。 寫入檔案時，會視需要配置非零資料的空間。

-   只有壓縮檔案或稀疏檔案可以有作業系統已知的零範圍。

-   如果檔案為稀疏或壓縮檔案，NTFS 可能會解除配置檔案內的磁碟空間。 這會將位元組範圍設定為零，而不會延伸檔案大小。

## <a name="BKMK_examples"></a>典型
若要將 C：\Temp 目錄中名為 Sample .txt 的檔案標示為稀疏，請輸入：

```
fsutil sparse setflag c:\temp\sample.txt 
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


