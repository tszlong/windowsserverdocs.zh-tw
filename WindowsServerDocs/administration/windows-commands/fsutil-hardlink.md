---
title: fsutil hardlink
description: Fsutil 硬連結命令的參考文章，會在現有檔案和新檔案之間建立永久連結。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 9d3782736686106b892f3b35e74573e4e162ae80
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037356"
---
# <a name="fsutil-hardlink"></a>fsutil hardlink

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

建立現有檔案與新檔案之間的永久連結。 硬連結是檔案的目錄專案。 每個檔案都可以被視為至少有一個永久連結。

在 NTFS 磁片區上，每個檔案都可以有多個永久連結，因此單一檔案可以出現在多個目錄中， (或甚至在具有不同名稱) 的相同目錄中。 因為所有連結都會參考相同的檔案，所以程式可以開啟任何連結並修改檔案。 只有當檔案的所有連結都已刪除之後，才會從檔案系統中刪除檔案。 建立永久連結之後，程式可以像任何其他檔案名一樣使用它。

## <a name="syntax"></a>語法

```
fsutil hardlink create <newfilename> <existingfilename>
fsutil hardlink list <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 建立 | 在現有的檔案與新檔案之間建立 NTFS 永久連結。  (NTFS 永久連結類似于 POSIX 硬連結。 )  |
| \<newfilename> | 指定您想要建立永久連結的檔案。 |
| \<existingfilename> | 指定您想要建立永久連結的檔案。 |
| list | 列出永久連結至 *檔案名*。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
