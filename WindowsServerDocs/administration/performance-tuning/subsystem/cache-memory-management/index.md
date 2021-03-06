---
title: 快取與記憶體管理員子系統的效能調整
description: 快取與記憶體管理員子系統的效能調整
ms.topic: landing-page
ms.author: pavel
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 8c5800dc0f4d55d7cb240496a304dd56b97077f1
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078015"
---
# <a name="performance-tuning-cache-and-memory-manager"></a>快取與記憶體管理員的效能調整

根據預設，Windows 會快取從磁碟讀取與寫入到磁碟的檔案資料。 這表示讀取作業會從系統記憶體中的區域 (稱為系統檔案快取) 讀取檔案資料，而不是從實體磁碟讀取。 同樣地，寫入作業會將檔案資料寫入到系統檔案快取而非寫入到磁碟，而且此類型的快取稱為回寫式快取。 快取是以個別檔案物件為單位來管理。 快取是由「快取管理員」的指示進行，「快取管理員」會在 Windows 執行期間持續運作。

系統檔案快取中的檔案資料會於作業系統指定的間隔寫入磁碟。 排清的分頁會存在於系統快取工作集 (當 FILE\_FLAG\_RANDOM\_ACCESS 已設定且檔案控制代碼未關閉時) 或位於待命清單上 (這些會成為可用記憶體的一部分)。

延遲寫入資料到檔案並將它保留在快取直到排清快取的原則稱為 Lazy Writing，而且它是由「快取管理員」於預先設定的時間間隔觸發。 檔案資料區塊排清的時間部分是以資料儲存在快取中的時間長度以及該資料自上次在讀取作業中存取之後的時間長度為基礎。 這可確保頻繁讀取的檔案資料將可儘可能地維持在系統檔案快取中供存取。

下圖顯示此檔案資料快取程序：

![檔案資料快取](../../media/perftune-guide-file-data-caching.png)

如上圖實心箭號所述，當「快取管理員」在檔案讀取作業期間第一次要求資料時，資料的 256 KB 區域會讀入系統位址空間中的 256 KB 快取位置。 使用者模式處理序接著會將此位置資料複製到其自己的位址空間。 當處理序完成其資料存取時，它會將修改的資料寫回系統快取中的相同位置，如處理序位址空間與系統快取之間的點箭號所示。 當「快取管理員」判斷在一段特定時間都不再需要該資料時，它會將修改的資料寫回磁碟上的檔案，如系統快取與磁碟之間的點箭號所示。

**本節內容：**

-   [快取與記憶體管理員潛在效能問題](troubleshoot.md)

-   [Windows Server 2016 中快取與記憶體管理員的改良功能](./improvements-in-windows-server.md)