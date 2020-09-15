---
title: 針對快取和記憶體管理員效能問題進行疑難排解
description: 針對 Windows Server 16 上的快取和記憶體管理員效能問題進行疑難排解
ms.topic: article
ms.author: pavel
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cb9aa4d9ec10b27e423da9d312b5d395c6e2e690
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077985"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>針對快取和記憶體管理員效能問題進行疑難排解

在 Windows Server 2012 之前，有兩個主要的潛在問題導致系統檔案快取成長，直到可用的記憶體在特定工作負載下幾乎耗盡為止。 當這種情況導致系統變慢時，您可以判斷伺服器是否遇到上述其中一個問題。


## <a name="counters-to-monitor"></a>要監視的計數器

-   記憶體 \\ 長期平均待命快取存留期 (s) &lt; 1800 秒

-   記憶體 \\ 可用 mb 偏低

-   記憶體 \\ 系統快取常駐位元組

如果記憶體 \\ 的可用 mb 數很低，而且記憶體系統快取 \\ 常駐位元組在實體記憶體中耗用大量的記憶體，您可以使用 [RAMMAP](/sysinternals/downloads/rammap) 來瞭解快取的用途。

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>系統檔案快取包含 NTFS 中繼檔資料結構


這個問題是由 RAMMAP 輸出中非常大量的使用中中繼檔頁面所表示，如下圖所示。 此問題可能已在存取數百萬個檔案的忙碌伺服器上觀察到，因而導致無法從快取中釋放 NTFS 中繼檔資料。

![rammap 視圖](../../media/perftune-guide-rammap.png)

*DynCache*工具用來減輕的問題。 在 Windows Server 2012 + 中，架構已經過重新設計，而且這個問題應該不再存在。

## <a name="system-file-cache-contains-memory-mapped-files"></a>System file cache 包含記憶體對應檔案


這個問題是由 RAMMAP 輸出中非常大量的使用中對應檔案頁面所表示。 這通常表示伺服器上的某些應用程式正在使用 [CreateFile](/windows/win32/api/fileapi/nf-fileapi-createfilea) API 搭配檔案 \_ 旗標 \_ 隨機存取旗標設定，來開啟大量的大型檔案 \_ 。

知識庫文章 [2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369)將詳細說明此問題。 檔案 \_ 旗標 \_ 隨機 \_ 存取旗標是一種提示，可讓快取管理員將檔案的對應視圖維持在記憶體中的最長時間， (直到記憶體管理員未發出) 記憶體不足的狀況。 同時，此旗標會指示快取管理員停用檔案資料的預先提取。

這種情況在 Windows Server 2012 + 的工作集修剪改進方面已經減輕了某些範圍，但是問題本身必須由應用程式廠商處理，而不是使用檔案 \_ 旗標 \_ 隨機 \_ 存取。 應用程式廠商的替代解決方案可能是在存取檔案時使用低記憶體優先順序。 您可以使用 [SetThreadInformation](/windows/win32/api/processthreadsapi/nf-processthreadsapi-setthreadinformation) API 來達成此目的。 以低記憶體優先權存取的頁面會更積極地從工作集移除。

從 Windows Server 2016 開始，快取管理員會在進行修剪決策時略過 FILE_FLAG_RANDOM_ACCESS，以進一步降低此風險，因此它的處理方式就如同任何其他未使用 FILE_FLAG_RANDOM_ACCESS 旗標而開啟的檔案 (快取管理員仍接受此旗標來停用預先提取的檔案資料) 。如果您有大量以這個旗標開啟的檔案，並以真正隨機的方式來存取，您仍然可以讓系統快取膨脹。 強烈建議您不要使用 FILE_FLAG_RANDOM_ACCESS 應用程式。