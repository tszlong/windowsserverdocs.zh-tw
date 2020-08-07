---
title: fsutil reparsepoint
description: Fsutil reparsepoint 命令的參考文章，它會查詢或刪除重新分析點。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b7742b7bb970394f0ef8602ae5c862c2ff9a1a41
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889883"
---
# <a name="fsutil-reparsepoint"></a>fsutil reparsepoint

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

查詢或刪除重新分析點。  **Fsutil reparsepoint**命令通常是由支援專業人員所使用。

重新分析點是具有可定義屬性的 NTFS 檔案系統物件，其中包含使用者定義的資料。 它們是用來：

- 擴充輸入/輸出 (i/o) 子系統中的功能。

- 作為目錄連接點和磁片區掛接點。

- 將特定檔案標記為檔案系統篩選器驅動程式的特殊檔案。

## <a name="syntax"></a>語法

```
fsutil reparsepoint [query] <filename>
fsutil reparsepoint [delete] <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 查詢 | 抓取與指定的控制碼所識別之檔案或目錄相關聯的重新分析點資料。 |
| delete | 從指定的控制碼所識別的檔案或目錄中刪除重新分析點，但不會刪除檔案或目錄。 |
| `<filename>` | 指定檔案的完整路徑，包括檔案名和副檔名，例如*C:\documents\filename.txt*。 |

#### <a name="remarks"></a>備註

- 當程式設定重新分析點時，它會儲存此資料，再加上可唯一識別所儲存資料的重新分析標記。 當檔案系統以重新分析點開啟檔案時，它會嘗試尋找相關聯的檔案系統篩選器。 如果找到檔案系統篩選器，篩選器會依照重新分析資料的指示來處理檔案。 如果找不到任何檔案系統篩選器，則檔案**開啟**作業會失敗。

### <a name="examples"></a>範例

若要取得與*c:\server*相關聯的重新分析點資料，請輸入：

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
