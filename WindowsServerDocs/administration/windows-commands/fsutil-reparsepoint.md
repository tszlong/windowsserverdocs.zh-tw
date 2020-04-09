---
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
title: Fsutil reparsepoint
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 0b819f15e473738996484283bceac439f482a13d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844151"
---
# <a name="fsutil-reparsepoint"></a>Fsutil reparsepoint
>適用于： Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7、Windows 2008、Windows Vista

查詢或刪除重新分析點。  **Fsutil reparsepoint**命令通常是由支援專業人員所使用。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

### <a name="parameters"></a>參數

| 參數  |                                                                描述                                                                |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|   query    |            抓取與指定的控制碼所識別之檔案或目錄相關聯的重新分析點資料。             |
|   [刪除]   | 從指定的控制碼所識別的檔案或目錄中刪除重新分析點，但不會刪除檔案或目錄。 |
| <FileName> |             指定檔案的完整路徑，包括檔案名和副檔名，例如 C:\documents\filename.txt。             |

## <a name="remarks"></a>備註

-   重新分析點是 NTFS 檔案系統物件，其具有可定義的屬性，其中包含使用者定義的資料，而且會用來擴充輸入/輸出（i/o）子系統中的功能。

-   重新分析點用於目錄連接點和磁片區掛接點。 檔案系統篩選器驅動程式也會使用這些檔案，將特定檔案標記為該驅動程式的特殊檔案。

-   當程式設定重新分析點時，它會儲存此資料，再加上可唯一識別所儲存資料的重新分析標記。 當檔案系統以重新分析點開啟檔案時，它會嘗試尋找相關聯的檔案系統篩選器。 如果找到檔案系統篩選器，篩選器會依照重新分析資料的指示來處理檔案。 如果找不到任何檔案系統篩選器，則檔案開啟作業會失敗。

## <a name="examples"></a><a name="BKMK_examples"></a>典型
若要取得與 C:\Server 相關聯的重新分析點資料，請輸入：

```
fsutil reparsepoint query c:\server
```

若要從指定的檔案或目錄中刪除重新分析點，請使用下列格式：

```
fsutil reparsepoint delete c:\server
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


