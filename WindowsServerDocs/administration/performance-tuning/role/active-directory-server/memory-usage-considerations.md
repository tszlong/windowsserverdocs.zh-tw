---
title: AD DS 效能微調中的記憶體使用量考慮
description: 執行 Windows Server 2012 R2、2016和2019之網域控制站上的 Lsass.exe 進程所使用的記憶體。
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: teresa-motiv
ms.date: 7/3/2019
ms.openlocfilehash: cceabd73a3064ff82cfe1d3c353ea63574f5feff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851881"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>AD DS 效能微調的記憶體使用量考慮

本文說明本機安全性授權子系統服務（LSASS，也稱為 Lsass.exe 進程）的一些基本概念、LSASS 設定的最佳作法，以及記憶體使用量的預期。 本文應該做為分析網域控制站（Dc）上的 LSASS 效能和記憶體使用的指南。 如果您有關于如何微調和設定伺服器和 Dc 以優化此引擎的問題，本文中的資訊可能會很有用。  

LSASS 負責管理本地安全機構（LSA）網域驗證與 Active Directory 管理。 LSASS 會處理用戶端和伺服器的驗證，而且也會控制 Active Directory 引擎。 LSASS 負責下列元件：  

- 本機安全性授權
- NetLogon 服務
- 安全性帳戶管理員（SAM）服務
- LSA 伺服器服務
- 安全通訊端層（SSL）
- Kerberos v5 驗證通訊協定
- NTLM 驗證通訊協定
- 載入 LSA 的其他驗證套件

Active Directory 資料庫服務（NTDSAI）可與可延伸儲存引擎（ESE、ESENT）搭配使用。

以下是 DC 上 LSASS 記憶體使用量的視覺化圖：

![使用 LSASS 記憶體之元件的圖表](media/domain-controller-lsass-memory-usage.png)  

LSASS 在 DC 上使用的記憶體數量會根據 Active Directory 使用量而增加。 查詢資料時，它會快取到記憶體中。 如此一來，使用大於 Active Directory 資料庫檔案（ntds.dit）大小的記憶體數量時，就會正常地看到 LSASS。

如圖中所示，LSASS 記憶體使用量可分成數個部分，包括 ESE 資料庫緩衝區快取、ESE 版本存放區，以及其他專案。 本文的其餘部分可讓您深入瞭解每個部分。

## <a name="ese-database-buffer-cache"></a>ESE 資料庫緩衝區快取  
LSASS 內最大的變數記憶體使用量是 ESE 資料庫緩衝區快取。 快取大小的範圍可從小於 1 MB 到整個資料庫的大小。 因為較大的快取可改善效能，所以 Active Directory （ESENT）的資料庫引擎會嘗試盡可能將快取保持在最大的大小。 雖然快取的大小因電腦的記憶體壓力而有所不同，但是 ESE 資料庫緩衝區快取的大小上限*只*受限於電腦上安裝的實體 RAM。 只要沒有其他記憶體不足的壓力，快取就會成長到 Active Directory 的 ntds.dit 資料庫檔案的大小。 可以快取的資料庫越多，DC 的效能就愈好。  
  
> [!NOTE]
> 由於資料庫快取演算法的運作方式，在資料庫大小小於可用 RAM 的64位系統上，資料庫快取的成長可能會比資料庫大小高達30到40%。

## <a name="ese-version-store"></a>ESE 版本存放區

ESE 版本存放區（上圖中的紅色部分）有變數記憶體使用量。 使用的記憶體數量取決於您是否有 Windows Server 2019 或舊版的 Windows。

- 在 predating Windows Server 2019 的 Windows Server 版本中，根據預設，LSASS 可能會針對 ESE 版本存放區使用64位電腦上最多的為400MB 記憶體（視 Cpu 數目而定）。 如需如何使用版本存放區的詳細資訊，請參閱 Ryan Ries 的下列請參閱 ASKDS blog 文章：[名為的版本存放區，而它們全都超出 bucket](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415)。

- 在 Windows Server 2019 中，這是簡化的，而且當 NTDS 服務第一次啟動時，ESE 版本存放區大小現在會計算為10% 的實體 RAM，最低為為400MB，最大值為 4 GB。 如需有關此和版本存放區疑難排解的絕佳詳細資料，請參閱 Ryan Ries 的另一個絕佳的 blog：[深入探討：在伺服器2019中 ACTIVE DIRECTORY ESE 版本存放區變更](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Deep-Dive-Active-Directory-ESE-Version-Store-Changes-in-Server/ba-p/400510)。

## <a name="other-memory-use"></a>其他記憶體使用量

最後，有程式碼、堆疊、堆積和各種固定大小的資料結構（例如，架構快取）。 根據電腦上的負載而定，LSASS 使用的記憶體數量可能會有所不同。 當執行中的執行緒數目增加時，記憶體堆疊的數目也會隨之增加。 平均而言，LSASS 會針對這些固定元件使用 100 MB 到 300 MB 的記憶體。 安裝較大量的 RAM 時，LSASS 可以使用更多的 RAM 和較少的虛擬記憶體。

**限制或減少網域控制站上的程式數目，或在適當時新增額外的 RAM**

為了達到最佳效能，LSASS 會盡可能在指定的 DC 上佔用最多的 RAM。 LSASS 會讓出其他進程所要求的 RAM。 其概念是將 LSASS 的效能優化，同時仍會計入可能會在電腦上執行的其他進程。 要監看的程式清單包含監視代理程式。 有些客戶針對各種伺服器功能各有不同的代理程式，這些函式可能會耗用大量的 RAM 資源。 有些可能會發出許多 WMI 查詢，我們會在下面提供一些詳細資料。

基於此原因，若要提升效能，最好將 DC 上的程式數目限制為或最小化。 如果沒有記憶體要求，LSASS 會使用此記憶體來快取 Active Directory 資料庫，因而達到最佳效能。

當您發現 DC 發生效能問題時，也請留意具有大量記憶體使用率的進程。 這些可能會有問題，您必須進行疑難排解。 他們可能包括 Microsoft 元件。 請確定您隨時掌握最新的服務更新&mdash;Microsoft 包含過多的記憶體使用量解決方案，做為品質更新的一部分，這也可能有助於您的 DC 效能。

根據使用設定檔而定，有內建的 OS 設備可能會耗用大量的 RAM：

- **檔案伺服器**。 Dc 也是適用于 SYSVOL 和 Netlogon 共用的檔案伺服器，負責原則和啟動/登入腳本的服務群組原則和腳本。
  不過，我們看到客戶使用 Dc 來服務其他檔案內容。 SMB 檔案伺服器接著會耗用 RAM 來追蹤作用中的用戶端，但最重要的是，檔案內容會使 OS 檔案快取成長，並與 ESE 資料庫快取競爭以提供 RAM。  

- **WMI 查詢**。 監視解決方案通常會進行許多 WMI 查詢。 個別查詢的執行可能會很便宜。 通常這是產生一些額外負荷的呼叫量，特別是當監視解決方案從 Windows 管理的各種事件記錄檔中解壓縮新事件時。  

  產生最多磁片區的事件記錄檔通常是安全性事件記錄檔。 這也是安全性系統管理員想要收集的事件記錄檔，特別是從 Dc。  

  WMI 服務會使用可優化查詢的「動態記憶體配置」配置。 因此，WMI 服務可能會配置大量的記憶體，並再次與 ESE 資料庫快取競爭。  
