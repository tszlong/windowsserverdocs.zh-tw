---
title: 針對快取和記憶體管理員的效能問題進行疑難排解
description: 針對快取和記憶體管理員在 Windows Server 16 上的效能問題進行疑難排解
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 66c7e2a6b264a837c65df927b271fadd2672fa24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835789"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>針對快取和記憶體管理員的效能問題進行疑難排解

Windows Server 2012 中前, 兩個主要的潛在問題會造成成長，直到可用的記憶體幾乎用盡數，在特定工作量下的系統檔案快取。 當這種情況會導致系統效能不彰，您可以決定是否伺服器會面臨這些問題的其中一個。


## <a name="counters-to-monitor"></a>若要監視的計數器

-   記憶體\\長期平均待機快取存留期 （秒） &lt; 1800 秒

-   記憶體\\Available Mbytes 偏低

-   記憶體\\系統快取常駐位元組

如果記憶體\\Available Mbytes 偏低 」 和 「 在此同時記憶體\\System Cache Resident Bytes 正在使用的實體記憶體的重要部分，您可以使用[RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx)若要了解正在使用哪些快取。

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>系統檔案快取包含 NTFS 中繼資料結構


此問題會以非常大量 active 中繼檔中的分頁 RAMMAP 輸出，如下圖所示。 這個問題可能會觀察到在忙碌的伺服器上有數百萬個檔案的存取，進而導致快取不從快取釋放 NTFS 中繼資料。

![rammap 檢視](../../media/perftune-guide-rammap.png)

問題來減輕*DynCache*工具。 在 Windows Server 2012 +，架構已經過重新設計並不應該存在這個問題。

## <a name="system-file-cache-contains-memory-mapped-files"></a>系統檔案快取包含記憶體對應檔案


此問題會以非常大量的作用中的對應檔分頁 RAMMAP 輸出中。 這通常表示在伺服器上的某些應用程式會開啟使用大型檔案大量[CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx)使用檔案 API\_旗標\_隨機\_存取旗標集。

知識庫文件中詳細說明本期[2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369)。 檔案\_旗標\_隨機\_存取旗標是用於快取管理員 （直到過了記憶體管理員並不表示記憶體不足的情況），檔案的對應的檢視保留在記憶體中盡可能的提示。 在此同時，這個旗標指示停用預先擷取的檔案資料的快取管理員。

藉由使用集修剪改進在 Windows Server 2012 +，這種情況減輕到某種程度來說，但需要主要應用程式廠商所處理，不使用檔案本身的問題\_旗標\_隨機\_存取。 替代方案的應用程式廠商可能會存取檔案時，請使用低記憶體優先順序。 這可以使用來達成[SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) API。 在低記憶體優先順序會存取的頁面會從工作集更積極地移除。

快取管理員 中，從 Windows Server 2016 進一步降低這進行調整的決策，因此它是被視為任何其他不 （快取管理員仍會接受這 FILE_FLAG_RANDOM_ACCESS 旗標所開啟的檔案時，請略過 FILE_FLAG_RANDOM_ACCESS旗標設為停用檔案資料的預先擷取）。 如果您有大量的檔案開啟這個旗標和存取真正的隨機的方式，您仍然可以讓系統快取膨脹。 強烈建議，FILE_FLAG_RANDOM_ACCESS 不使用這個應用程式。
