---
title: fsutil hardlink
description: Fsutil hard hard 命令的參考文章，它會在現有檔案和新檔案之間建立永久連結。
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b4cbc3912339464a061c027234d0d22b2d73ea09
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932280"
---
# <a name="fsutil-hardlink"></a>fsutil hardlink

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

建立現有檔案與新檔案之間的永久連結。 「硬連結」是檔案的目錄專案。 每個檔案都可以被視為至少有一個硬連結。

在 NTFS 磁片區上，每個檔案都可以有多個永久連結，因此單一檔案可能會出現在許多目錄中（或甚至在具有不同名稱的相同目錄中）。 由於所有連結都參考相同的檔案，因此程式可以開啟任何連結並修改檔案。 只有在刪除檔案的所有連結之後，檔案才會從檔案系統中刪除。 建立硬式連結之後，程式就可以像任何其他檔案名一樣使用它。

## <a name="syntax"></a>語法

```
fsutil hardlink create <newfilename> <existingfilename>
fsutil hardlink list <filename>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 建立 | 在現有檔案和新檔案之間建立 NTFS 永久連結。 （NTFS 硬連結類似 POSIX 硬連結）。 |
| \<newfilename> | 指定您想要建立硬連結的檔案。 |
| \<existingfilename> | 指定您想要建立硬連結的檔案。 |
| list | 列出*檔案名*的永久連結。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
