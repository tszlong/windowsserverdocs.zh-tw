---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: fsutil 疏鬆
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b1bc4e45ed2a2b06c72318e0999988ed8f016c40
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438976"
---
# <a name="fsutil-sparse"></a>fsutil 疏鬆
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

管理疏鬆檔案。

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
|     queryflag     |                                                  疏鬆的查詢。                                                  |
|    queryrange     |                        掃描檔案，並搜尋範圍可能包含非零的資料。                        |
|      setflag      |                                        將指定為疏鬆檔案的標記。                                        |
|     setrange      |                                   指定的範圍的檔案中填入零。                                   |
|    <FileName>     | 指定包含檔案名稱和副檔名，例如 C:\documents\filename.txt 檔案的完整路徑。 |
| <BeginningOffset> |                              指定要標記為疏鬆檔案中的位移。                              |
|     <Length>      |                 指定長度的區域檔案中標示為疏鬆 （以位元組為單位）。                 |

## <a name="remarks"></a>備註

-   疏鬆檔案是具有一或多個區域中的未配置資料的檔案。 程式會查看這些未配置的區域，為包含零值的位元組，但沒有磁碟空間用來代表這些零。 配置有意義，或非零值的所有資料，而不被都配置所有 nonmeaningful 資料 （資料由零組成的大型字串）。 疏鬆檔案讀取時，會傳回配置的資料，因為儲存，和未配置的資料，根據預設，以傳回零，根據 C2 安全性需求規格。 疏鬆檔案支援可取消配置從任何位置在檔案中的資料。

-   在疏鬆檔案中，大範圍的零可能不需要的磁碟配置。 視需要寫入檔案時，將會配置空間給非零的資料。

-   只壓縮或疏鬆檔案可以有零作業系統已知的範圍。

-   如果檔案是疏鬆或壓縮，NTFS 可能會解除配置檔案中的磁碟空間。 這會設定為零，而不需要擴充的檔案大小的位元組範圍。

## <a name="BKMK_examples"></a>範例
若要標記為疏鬆的 C:\Temp 目錄中名為 Sample.txt 檔案，請輸入：

```
fsutil sparse setflag c:\temp\sample.txt 
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


