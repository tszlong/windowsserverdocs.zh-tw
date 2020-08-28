---
title: fsutil reparsepoint
description: Fsutil reparsepoint 命令的參考文章，可查詢或刪除重新分析點。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 94feb56f4c5ed590c87b20d475f4aefa7ba11dd4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037316"
---
# <a name="fsutil-reparsepoint"></a>fsutil reparsepoint

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

查詢或刪除重新分析點。  支援專業人員通常會使用 **fsutil reparsepoint** 命令。

重新分析點是 NTFS 檔案系統物件，具有可定義的屬性，其中包含使用者定義的資料。 它們是用來：

- 在輸入/輸出 (i/o) 子系統中擴充功能。

- 作為目錄連接點和磁片區掛接點。

- 將某些檔案標示為特殊的檔案系統篩選器驅動程式。

## <a name="syntax"></a>語法

```
fsutil reparsepoint [query] <filename>
fsutil reparsepoint [delete] <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 查詢 | 抓取與指定之控制碼所識別之檔案或目錄相關聯的重新分析點資料。 |
| delete | 從指定之控制碼所識別的檔案或目錄中刪除重新分析點，但不會刪除檔案或目錄。 |
| `<filename>` | 指定檔案的完整路徑，包括檔案名和副檔名，例如 *C:\documents\filename.txt*。 |

#### <a name="remarks"></a>備註

- 當程式設定重新分析點時，它會儲存這項資料，加上重新分析標記，可唯一識別它所儲存的資料。 當檔案系統開啟具有重新分析點的檔案時，它會嘗試尋找相關聯的檔案系統篩選器。 如果找到檔案系統篩選器，則篩選會依重新分析資料的指示處理檔案。 如果找不到任何檔案系統篩選器，則檔案 **開啟** 作業會失敗。

### <a name="examples"></a>範例

若要取出與 *c:\server*相關聯的重新分析點資料，請輸入：

```
fsutil reparsepoint query c:\server
```

若要從指定的檔案或目錄中刪除重新分析點，請使用下列格式：

```
fsutil reparsepoint delete c:\server
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
