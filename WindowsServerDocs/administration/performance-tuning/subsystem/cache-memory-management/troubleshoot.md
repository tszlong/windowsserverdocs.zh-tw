---
title: 針對快取和記憶體管理員效能問題進行疑難排解
description: 針對 Windows Server 16 上的快取和記憶體管理員效能問題進行疑難排解
ms.topic: article
ms.author: pavel; atales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: ba6c04ca8ac1941221809af958a9d6c53701b101
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992018"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>針對快取和記憶體管理員效能問題進行疑難排解

在 Windows Server 2012 之前，有兩個主要的潛在問題造成系統檔案快取成長，直到在特定工作負載下的可用記憶體用盡。 當這種情況導致系統變慢時，您可以判斷伺服器是否遇到這些問題的其中一個。


## <a name="counters-to-monitor"></a>要監視的計數器

-   記憶體 \\ 長期平均待命快取存留期 (s) &lt; 1800 秒

-   記憶體 \\ 可用 mb 偏低

-   記憶體 \\ 系統快取常駐位元組

如果記憶體 \\ 可用 mb 偏低，而且記憶體系統快取 \\ 常駐位元組耗用實體記憶體的重要部分，您可以使用[RAMMAP](/sysinternals/downloads/rammap)來找出快取的用途。

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>系統檔案快取包含 NTFS 中繼檔資料結構


這個問題是由 RAMMAP 輸出中非常大量的使用中中繼檔頁面所表示，如下圖所示。 此問題可能是在有數百萬個檔案被存取的忙碌伺服器上觀察到，因而導致不會從快取中釋放 NTFS 中繼檔資料。

![rammap 視圖](../../media/perftune-guide-rammap.png)

*DynCache*工具所用的問題。 在 Windows Server 2012 + 中，架構已經過重新設計，此問題應該不會再存在。

## <a name="system-file-cache-contains-memory-mapped-files"></a>系統檔案快取包含記憶體對應檔案


此問題是以 RAMMAP 輸出中非常大量的使用中對應檔案頁面來表示。 這通常表示伺服器上的某些應用程式正在使用[CreateFile](/windows/win32/api/fileapi/nf-fileapi-createfilea) API 開啟許多大型檔案，並已 \_ 設定檔案旗標 \_ 隨機 \_ 存取旗標。

此問題會在 KB 文章[2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369)中詳細說明。 檔案 \_ 旗標 \_ 隨機 \_ 存取旗標是一種提示，可讓快取管理員在記憶體中保持檔案的對應視圖，直到記憶體管理員未發出) 記憶體不足的狀況 (為止。 同時，此旗標會指示快取管理員停用檔案資料的預先提取。

這種情況已透過 Windows Server 2012 + 中的工作集調整改善而降低到某個程度，但問題本身必須由應用程式廠商解決，而不是使用檔案 \_ 旗標 \_ 隨機 \_ 存取。 應用程式廠商的替代方案可能是在存取檔案時使用低記憶體優先順序。 這可以使用[SetThreadInformation](/windows/win32/api/processthreadsapi/nf-processthreadsapi-setthreadinformation) API 來達成。 低記憶體優先順序存取的頁面會更積極地從工作集中移除。

從 Windows Server 2016 開始的快取管理員，會在進行修剪決策時略過 FILE_FLAG_RANDOM_ACCESS，藉此減少此情況，因此它的處理方式就如同在沒有 FILE_FLAG_RANDOM_ACCESS 旗標的情況下開啟的任何其他檔案 (快取管理員仍接受此旗標來停用預先提取檔案資料) 如果您有大量的檔案以此旗標開啟，而且以真正隨機的方式存取，仍然可以造成系統快取膨脹。 強烈建議不要讓應用程式使用 FILE_FLAG_RANDOM_ACCESS。