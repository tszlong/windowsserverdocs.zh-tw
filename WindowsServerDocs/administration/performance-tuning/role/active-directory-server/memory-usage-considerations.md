---
title: 在 AD DS 效能微調記憶體使用狀況的考量
description: Lsass.exe 程序正在執行 Windows Server 2012 R2，2016年和 2019年的網域控制站上的記憶體使用量。
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: dd33124d7d480f3670fa2781f567eaaa28abbce6
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799780"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>對於 AD DS 效能微調的記憶體使用方式考量

本文說明一些基本的本機安全性授權子系統服務 (LSASS 也稱為 Lsass.exe 處理程序)，LSASS 的組態的最佳做法和記憶體使用量的期望。 這篇文章應該使用網域控制站 (Dc) 上的 LSASS 效能和記憶體使用率分析的指南。 這篇文章中的資訊可能很有用，如果您有關於如何微調和設定伺服器和網域控制站以最佳化此引擎的問題。  

LSASS 負責和 Active Directory 管理本機安全性授權 (LSA) 網域驗證的管理。 LSASS 處理用戶端和伺服器驗證，也可控制 Active Directory 的引擎。 LSASS 會負責下列元件：  

- 本機安全性授權
- NetLogon 服務
- 安全性帳戶管理員 (SAM) 服務
- LSA 伺服器服務
- 安全通訊端層 (SSL)
- Kerberos v5 驗證通訊協定
- NTLM 驗證通訊協定
- 載入 LSA 其他驗證封裝

Active Directory 資料庫服務 (NTDSAI.dll) 使用可延伸儲存引擎 （ESE、 ESENT.dll）。

以下是在 DC 上的 LSASS 記憶體使用量的視覺化圖表：

![使用 LSASS 記憶體元件的圖表](media/domain-controller-lsass-memory-usage.png)  

LSASS 會使用在 DC 的記憶體數量會增加根據 Active Directory 使用方式。 查詢資料時，它會在記憶體中快取。 如此一來，它是記憶體的正常的請參閱 LSASS 使用 Active Directory 資料庫 (NTDS.dit) 檔案的大小數量。

如圖所示，LSASS 記憶體使用量可以分成幾個部分，包括 ESE 資料庫緩衝快取、 ESE 版本儲存區等等。 這篇文章的其餘部分會提供深入了解每個部分。

## <a name="ese-database-buffer-cache"></a>ESE 資料庫緩衝快取  
LSASS 中的最大變數的記憶體使用量是 ESE 資料庫緩衝快取。 快取大小的範圍可以從小於 1 MB 到整個資料庫的大小。 較大的快取可改善效能，因為資料庫引擎的 Active Directory (ESENT) 會嘗試保留快取一樣大。 雖然快取的大小會隨著電腦中的記憶體壓力，ESE 資料庫緩衝快取的大小上限是*只*受到實體電腦上安裝的 RAM。 只要沒有任何其他記憶體不足的壓力，快取可以成長到 Active Directory NTDS.dit 資料庫檔案的大小。 更多資料庫可以快取的較佳效能的 dc 會變。  
  
> [!NOTE]
> 因為資料庫快取演算法的運作方式，在資料庫的大小是小於可用的 RAM，64 位元系統上資料庫快取可以成長到大於資料庫的大小 30 到 40%。

## <a name="ese-version-store"></a>ESE 版本儲存區

ESE 版本儲存區 （在上圖中紅色的部分） 沒有變數的記憶體使用量。 使用的記憶體數量取決於您是否有 Windows Server 2019 或舊版的 Windows。

- 在 Windows Server 版本中 predating Windows Server 2019，預設 LSASS 可能最多約 400 MB 的記憶體 （取決於 Cpu 數目） 的電腦使用 64 位元的 ESE 版本儲存。 版本存放區的使用方式的詳細資訊，請參閱下列的 ASKDS 部落格文章 Ryan Ries:[版本存放區呼叫，而且它們所有不貯體](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415)。

- 在 Windows Server 2019，這已經過簡化，當 NTDS 服務第一次啟動時，ESE 版本存放區的大小現在會計算為 400 MB 的最少和最大值 4 GB 的實體 RAM 的 10%。 絕佳的詳細資訊和版本存放區進行疑難排解，請參閱 Ryan Ries 從另一個很棒的部落格：[深入探討：Active Directory ESE 版本存放區變更伺服器 2019年](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Deep-Dive-Active-Directory-ESE-Version-Store-Changes-in-Server/ba-p/400510)。

## <a name="other-memory-use"></a>其他記憶體使用量

最後，還有程式碼、 堆疊、 堆積和各種不同的固定的大小資料結構 （例如，結構描述快取）。 LSASS 會使用的記憶體數量而有所不同，視電腦上的負載而定。 當執行中執行緒的數目增加時，會跟著數量的記憶體堆疊。 平均而言，LSASS 會針對這些固定的元件，使用 100 MB 到 300 MB 的記憶體。 安裝時，更大量的 RAM，LSASS 就可以使用更多的 RAM 和較少的虛擬記憶體。

**限制或最小化您的網域控制站上的應用程式的數目視情況加入額外的 RAM**

為了取得最佳效能，LSASS 會盡可能在指定的網域控制站需要一樣多的 RAM。 LSASS 會讓出的 RAM，因為其他處理序要求它。 其概念是最佳化 LSASS 的同時仍 accounting 的電腦上可能會執行其他處理序的效能。 要注意的程式清單包含監視代理程式。 有些客戶會有不同的代理程式的各種伺服器函式可能會耗用相當多的 RAM 資源。 有些可能會發出許多 WMI 查詢，我們有下列幾個詳細資料。

基於這個原因，並提升效能，最好限制，或在 DC 上的程式數目降至最低。 如果沒有記憶體要求，LSASS 就會使用此記憶體快取 Active Directory 資料庫，並因此達到最佳效能。

當您察覺 DC 發生效能問題時，還需要留意嚴重的記憶體使用率的處理序。 這些可能有的問題，您需要進行疑難排解。 它們可能會包含 Microsoft 元件。 請確定您掌握最新的服務更新&mdash;Microsoft 包含記憶體使用率過高的品質更新，這也有助於您的 DC 效能一部分的解決方案。

有可能會耗用大量的 RAM，視使用的設定檔的內建作業系統功能：

- **檔案伺服器**。 Dc 也會用於 SYSVOL 與 Netlogon 共用，服務群組原則和原則和啟動/登入指令碼的指令碼的檔案伺服器。
  不過，我們看到客戶使用服務的其他檔案內容的網域控制站。 SMB 檔案伺服器會再耗用 RAM 來追蹤使用中的用戶端，但首先，檔案內容會讓作業系統檔案快取變得，以及對 ESE 資料庫快取的競爭的 RAM。  

- **WMI 查詢**。 監視解決方案通常會進行許多的 WMI 查詢。 個別查詢可能執行的成本低廉。 通常很的磁碟區的呼叫會產生一些額外負荷，特別是監視解決方案從各種 Windows 管理的事件記錄檔中擷取新的事件。  

  事件記錄檔產生大部分的磁碟區通常是安全性事件記錄檔。 而這也是安全性系統管理員想要收集，尤其是從網域控制站的事件記錄檔。  

  WMI 服務會使用動態記憶體配置方案的最佳化查詢。 因此，WMI 服務可能會配置大量記憶體，ESE 資料庫快取的競爭，一次。  
