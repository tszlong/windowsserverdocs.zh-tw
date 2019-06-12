---
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
title: Fsutil reparsepoint
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: f66f09fa608fec10d7126e516f9cf2dd8a19bbfb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438991"
---
# <a name="fsutil-reparsepoint"></a>Fsutil reparsepoint
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7、 Windows 2008，Windows Vista

查詢或刪除重新分析點。  **Fsutil reparsepoint**命令通常會由支援專業人員使用。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

## <a name="parameters"></a>參數

| 參數  |                                                                描述                                                                |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|   查詢    |            擷取的檔案或指定的控制代碼所識別的目錄相關聯的重新分析點資料。             |
|   [刪除]   | 從檔案或目錄由指定的控制代碼，但不會刪除檔案或目錄中刪除重新分析點。 |
| <FileName> |             指定包含檔案名稱和副檔名，例如 C:\documents\filename.txt 檔案的完整路徑。             |

## <a name="remarks"></a>備註

-   重新分析點是 NTFS 檔案系統物件具有可定義的屬性，其中包含使用者定義的資料，以及它們用來擴充功能中的輸入/輸出 (I/O) 子系統。

-   重新分析點可用於目錄掛接點以及磁碟區掛接點。 它們也會使用檔案系統篩選器驅動程式將標示為特殊的驅動程式特定檔案。

-   當程式設定重新分析點時，它會儲存此資料，再加上唯一識別它儲存資料的重新分析標記。 當檔案系統開啟檔案以重新分析點時，它會嘗試尋找相關聯的檔案系統篩選器。 如果找到檔案系統篩選器，篩選條件會處理檔案的重新分析資料的指示。 如果不找到任何檔案系統篩選器，則檔案的開啟作業將會失敗。

## <a name="BKMK_examples"></a>範例
若要擷取 C:\Server 相關聯的重新分析點資料，請輸入：

```
fsutil reparsepoint query c:\server
```

若要從指定的檔案或目錄刪除重新分析點，請使用下列格式：

```
fsutil reparsepoint delete c:\server
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


