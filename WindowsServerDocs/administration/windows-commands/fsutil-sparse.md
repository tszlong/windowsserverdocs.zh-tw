---
title: fsutil sparse
description: 用於管理稀疏檔案之 fsutil sparse 命令的參考文章。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: d79144d3894e9e181ebd889ce7bf281b827dea26
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889832"
---
# <a name="fsutil-sparse"></a>fsutil sparse

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

管理稀疏檔案。 「稀疏檔案」（sparse file）是一個檔案，其中包含一或多個未配置資料的區域。

程式會將這些未配置的區域視為包含零值的位元組，而且沒有代表這些零的磁碟空間。 當讀取稀疏檔案時，配置的資料會以預存的形式傳回，而且根據 C2 安全性需求規格，預設會將未配置的資料傳回為零。 稀疏檔案支援可讓資料從檔案中的任何位置解除配置。

## <a name="syntax"></a>語法

```
fsutil sparse [queryflag] <filename>
fsutil sparse [queryrange] <filename>
fsutil sparse [setflag] <filename>
fsutil sparse [setrange] <filename> <beginningoffset> <length>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| queryflag | 查詢稀疏。 |
| queryrange | 掃描檔案，並搜尋可能包含非零資料的範圍。 |
| setflag | 將指定的檔案標示為 sparse。 |
| setrange | 以零填滿指定的檔案範圍。 |
| `<filename>` | 指定檔案的完整路徑，包括檔案名和副檔名，例如*C:\documents\filename.txt*。 |
| `<beginningoffset>` | 指定要標記為稀疏的檔案中的位移。 |
| `<length>` | 指定檔案中要標示為稀疏 (的區域長度（以位元組為單位）) 。 |

#### <a name="remarks"></a>備註

- 會配置所有有意義或非零的資料，而所有不具意義的資料 (由零) 組成的大型資料字串，則不會進行配置。

- 在稀疏檔案中，大型的零範圍可能不需要磁片配置。 當寫入檔案時，會視需要配置非零資料的空間。

- 只有壓縮檔案或稀疏檔案可以有作業系統已知的零範圍。

- 如果檔案為稀疏或壓縮檔案，NTFS 可能會在檔案內解除配置磁碟空間。 這會將位元組範圍設定為零，而不會延伸檔案大小。

### <a name="examples"></a>範例

若要將*c：\temp*目錄中名為*sample.txt*的檔案標示為稀疏，請輸入：

```
fsutil sparse setflag c:\temp\sample.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
