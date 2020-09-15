---
title: AD DS 效能微調中的記憶體使用量考慮
description: 在執行 Windows Server 2012 R2、2016和2019的網域控制站上，Lsass.exe 進程的記憶體使用量。
ms.topic: article
ms.author: v-tea
author: teresa-motiv
ms.date: 7/3/2019
ms.openlocfilehash: 546839b803e8306fb3093ade6e317a155b7a117d
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077275"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>AD DS 效能調整的記憶體使用量考慮

本文說明本機安全性授權子系統服務 (LSASS 的一些基本概念，也就是 Lsass.exe 程式) 、LSASS 設定的最佳作法，以及記憶體使用量的預期。 本文應作為分析 (Dc) 的網域控制站上的 LSASS 效能和記憶體使用的指南。 如果您有關于如何微調和設定伺服器和 Dc 以將此引擎優化的問題，本文中的資訊可能會很有用。

LSASS 負責管理本地安全機構 (LSA) 網域驗證和 Active Directory 管理。 LSASS 會處理用戶端和伺服器的驗證，也會控制 Active Directory 引擎。 LSASS 負責下列元件：

- 本機安全性授權
- NetLogon 服務
- 安全性帳戶管理員 (SAM) 服務
- LSA 伺服器服務
- 安全通訊端層 (SSL) (Secure Sockets Layer (SSL))
- Kerberos v5 驗證通訊協定
- NTLM 驗證通訊協定
- 載入至 LSA 的其他驗證套件

Active Directory 資料庫服務 ( # A0) 使用可延伸儲存引擎 (ESE、ESENT.dll) 。

以下是 DC 上 LSASS 記憶體使用量的視覺化圖表：

![使用 LSASS 記憶體的元件圖表](media/domain-controller-lsass-memory-usage.png)

LSASS 在 DC 上使用的記憶體數量會隨著 Active Directory 的使用量增加。 當查詢資料時，它會快取在記憶體中。 如此一來，使用大於 Active Directory 資料庫檔案大小 (ntds.dit) 的記憶體數量就會是正常的。

如圖所示，LSASS 記憶體使用量可以分成數個部分，包括 ESE 資料庫緩衝區快取、ESE 版本存放區，以及其他部分。 本文的其餘部分可讓您深入瞭解每個元件。

## <a name="ese-database-buffer-cache"></a>ESE 資料庫緩衝區快取
LSASS 內的最大變數記憶體使用量是 ESE 資料庫緩衝區快取。 快取的大小可以從小於 1 MB 的範圍，到整個資料庫的大小。 因為較大的快取可改善效能，所以 Active Directory 的 database engine (ESENT) 會盡可能地嘗試保持快取的大小。 雖然快取的大小隨著電腦的記憶體壓力而異，但 ESE 資料庫緩衝區快取的大小上限 *只* 受電腦上安裝的實體 RAM 所限制。 只要沒有其他記憶體壓力，快取就可以成長到 Active Directory 的 ntds.dit 資料庫檔案的大小。 可以快取的資料庫越多，DC 的效能就越好。

> [!NOTE]
> 由於資料庫快取演算法的運作方式，因此，在資料庫大小小於可用 RAM 的64位系統上，資料庫快取的成長可能會超過30到40% 的資料庫大小。

## <a name="ese-version-store"></a>ESE 版本存放區

ESE 版本存放區會有變數記憶體使用量 () 上圖中的紅色部分。 使用的記憶體數量取決於您是否有 Windows Server 2019 或舊版的 Windows。

- 在 predating Windows Server 2019 的 Windows Server 版本中，根據預設，LSASS 可能會使用最多400MB 的記憶體 (，視適用于 ESE 版本存放區的64位電腦上) 的 Cpu 數目而定。 如需有關如何使用版本存放區的詳細資訊，請參閱 Ryan Ries 的下列 ASKDS blog 文章： [呼叫的版本存放區，而且全都超出值區](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415)。

- 在 Windows Server 2019 中，這已簡化，而且當 NTDS 服務初次開機時，ESE 版本存放區大小現在會計算為實體 RAM 的10%，最少為400MB，最大值為 4 GB。 如需此版本和版本存放區疑難排解的絕佳詳細資訊，請參閱 Ryan Ries 的另一篇絕佳的 blog： [深入探討：在伺服器2019中 ACTIVE DIRECTORY ESE 版本存放區變更](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Deep-Dive-Active-Directory-ESE-Version-Store-Changes-in-Server/ba-p/400510)。

## <a name="other-memory-use"></a>其他記憶體使用量

最後，有程式碼、堆疊、堆積和各種固定大小的資料結構 (例如，架構快取) 。 根據電腦上的負載而定，LSASS 使用的記憶體數量可能會有所不同。 當執行中的執行緒數目增加時，記憶體堆疊的數目就會增加。 根據平均，LSASS 針對這些固定的元件使用 100 MB 至 300 MB 的記憶體。 安裝較大量的 RAM 時，LSASS 可以使用更多的 RAM 和較少的虛擬記憶體。

**限制或最小化網域控制站上的程式數目，或在適當的情況下新增額外的 RAM**

為了達到最佳效能，LSASS 會盡可能地在指定的 DC 上採用最多的 RAM。 LSASS 會會讓出該 RAM 與其他進程的要求。 其概念是將 LSASS 的效能優化，同時仍會計入可能在電腦上執行的其他進程。 要監看的程式清單包含監視代理程式。 有些客戶會針對各種可能耗用大量 RAM 資源的伺服器功能使用不同的代理程式。 有些可能會發出許多 WMI 查詢，以下有幾個詳細資料。

因此，為了提高效能，最好是限制或減少 DC 上的程式數目。 如果沒有記憶體要求，LSASS 會使用此記憶體來快取 Active Directory 的資料庫，因此可達到最佳效能。

當您注意到 DC 有效能問題時，也請留意具有大量記憶體使用率的進程。 這些可能會有您需要進行疑難排解的問題。 它們可能包含 Microsoft 元件。 請確定您掌握最新的服務更新 &mdash; ，Microsoft 在品質更新中包含過多的記憶體使用量解決方案，這也可能有助於您的 DC 效能。

根據使用設定檔，有內建的 OS 設備可耗用大量 RAM：

- **檔案伺服器**。 Dc 也是適用于 SYSVOL 和 Netlogon 共用的檔案伺服器、服務群組原則和原則和啟動/登入腳本的腳本。
  不過，我們會看到客戶使用 Dc 來服務其他檔案內容。 然後，SMB 檔案伺服器會取用 RAM 來追蹤作用中的用戶端，但最重要的是，檔案內容會使 OS 檔案快取成長，並與 ESE 資料庫快取以進行 RAM 的競爭。

- **WMI 查詢**。 監視解決方案通常會進行許多的 WMI 查詢。 執行個別查詢可能會很便宜。 通常，這會產生一些額外負荷的呼叫量，尤其是監視解決方案會從 Windows 管理的各種事件記錄檔中提取新的事件。

  產生最多磁片區的事件記錄檔通常是安全性事件記錄檔。 這也是安全性系統管理員想要收集的事件記錄檔，尤其是 Dc。

  WMI 服務會使用可將查詢優化的動態記憶體配置配置。 因此，WMI 服務可能會配置大量的記憶體，再次與 ESE 資料庫快取競爭。
