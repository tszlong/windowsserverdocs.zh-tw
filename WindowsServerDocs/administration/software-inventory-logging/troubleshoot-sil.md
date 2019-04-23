---
title: 對軟體清查記錄進行疑難排解
description: 描述如何解決常見的軟體清查記錄部署問題。
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
author: brentfor
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 6ea8a336e2c40b55e2ad6b508d89db7dcb668315
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832849"
---
# <a name="troubleshoot-software-inventory-logging"></a>對軟體清查記錄進行疑難排解 

>適用於：Windows Server （半年通道），Windows Server 2019，Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012、 Windows Server 2008 R2

##<a name="understanding-sil"></a>了解 SIL

在開始進行疑難排解 SIL 之前，您應該充分了解其元件以及其運作方式。 下列影片提供概略說明 SIL 和 SIL 彙總工具，以及如何使用它們來轉送以及報告清查資料：

1. [軟體清查記錄 (SIL) (10:57) 簡介](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [軟體清查記錄：SIL 彙總工具 (14:34) 設定](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [軟體清查記錄：啟用 SIL 轉送 (7:20)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

##<a name="how-sil-data-flow-works"></a>SIL 資料流動的運作方式

SIL framework 具有兩個主要元件以及兩個通道的通訊。 透過兩者的資料流通道，並介於這兩個元件，的部署所需的成功 SIL （這假設的虛擬化，或雲端，純粹實體環境只需要一個通訊通道的環境-）。 您必須了解元件和資料流程的 SIL 將正確。 看看上述的概觀影片，您會看到說明這兩個通道上的元件和資料流程的架構圖。 橘色箭號表示透過 WinRM 的遠端查詢，綠色的虛線的箭頭表示 HTTPS 從 SIL SIL 彙總工具會每個 WS 結束節點：

![](../media/software-inventory-logging/image1.png)

如果您遇到 SIL 發生問題時，它很可能與的資料流程中的中斷透過的通道，以及元件之間。 以下是最常見的問題與資料流程下, 一節後面接著以解決每三個問題的疑難排解步驟：

-   **問題 1 資料流程**–**報表時使用發行 SilReport cmdlet 中的沒有資料**（或通常會遺失資料）。

-   **問題 2 資料流程**–**太未知的主機 下方的許多伺服器**報表中。

-   **問題 3 資料流程**–**太底下列為 未知的 OS 的實體主機的許多 Vm**在報表中，及/或使用時產生錯誤**Publish-sildata**執行 SIL 的 Windows 伺服器上。

##<a name="troubleshooting-data-flow-issues"></a>資料流問題進行疑難排解

在開始之前，您必須知道何時 SIL 彙總工具開始使用**開始 SilAggregator** cmdlet。

>[!IMPORTANT]
>會有任何資料在報表中的 SQL 資料 cube 處理在上午 3 點本機系統時間之前。 請勿繼續進行疑難排解步驟，直到處理 cube 資料。

如果您正在疑難排解在報表 （或從報表遺漏） 比上次還新的資料 cube 處理，或者 （適用於新安裝），曾經處理 cube 之前，請遵循下列步驟來處理即時的 SQL 資料 cube:

1. SQL Server 的系統管理員身分登入並執行**SSMS**在命令提示字元。
2. 連線至資料庫引擎。
3. 依序展開**SQL Server Agent** ，然後展開**作業**。
4. 以滑鼠右鍵按一下**SILStagingRefresh** ，然後選取**下列步驟啟動作業**。
5. 按一下 **啟動**並等待重新整理進度列完成。
6. 開啟系統管理員身分執行的 PowerShell**發佈 silreport openreport 巨集**cmdlet。

如果不仍在報表中的任何資料，請繼續進行三個資料流問題的疑難排解。

###<a name="data-flow-issue-1"></a>資料流問題 1 

####<a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>使用發行 SilReport cmdlet 時，報表中的沒有資料 （或遺漏資料通常）

如果資料遺失時，它可能是因為有尚未處理的 SQL 資料 cube。 如果它最近曾處理過，而且您認為遺漏的資料應該已抵達之前 cube 處理的彙總工具，請以反向順序遵循資料的路徑。 選擇唯一的主機及唯一的 VM 進行疑難排解。 反向的資料路徑會是**SILA 報表** &lt; **SILA 資料庫** &lt; **SILA 本機目錄** &lt; **遠端實體主機**或是**WS VM 執行 SIL 的代理程式/工作**。

####<a name="check-to-see-if-data-is-in-the-database"></a>請檢查資料是否在資料庫中

有兩種方式可檢查有資料：**Powershell**或是**SSMS**。

>[!Important]
>如果因為 SILA 插入資料庫中的資料，有至少一次處理 cube，此資料應該會反映在報表中。 如果在資料庫中沒有資料然後輪詢的實體主機失敗，或執行任何動作正在接收透過 HTTPS，或兩者。

 ####<a name="powershell"></a>PowerShell

1. 開啟系統管理員身分執行的 PowerShell **get silvmhost** cmdlet，然後執行**get silaggregator**。

    >[!NOTE]
    >輸出**get silaggregator**一律會模仿**Windows Server 詳細資料 索引標籤**的 Excel 報表。 非預期的結果不同。

2. Run **get-silvmhost**
 - 如果那里不列出任何主機，然後加入使用主機**新增 silvmhost** cmdlet。
 - 如果為列出的主機**未知**，然後移至問題 2。 d-如果列出的主機，但沒有沒有**datetime**下方**最近輪詢**資料行，然後移至**問題 2**如下。

**其他相關的命令**

**取得 SilAggregator-Computername&lt;將資料推送的已知伺服器 fqdn&gt;**:即使在處理 cube 之前，這會產生資料庫的電腦 (VM) 的相關資訊。 因此，此 cmdlet 可用來檢查資料庫中的資料推入 SIL 資料，透過 HTTPS，或不使用 cube 處理序，在上午 3 點 （或如果您還沒有重新整理 cube 中即時本節開頭所述） 的 Windows 伺服器。

**取得 SilAggregator VmHostName&lt;輪詢的實體主機的 fqdn 值的地方最近輪詢 資料行底下使用 Get-silvmhost cmdlet 時&gt;**:即使在處理 cube 之前，這會產生資料庫實體主機的相關資訊。

####<a name="ssms"></a>SSMS

n**檢查是否有資料從輪詢的主機：**
 
1. 開啟**SSMS** ，並連接到**Database Engine**。
2. 依序展開**資料庫**，展開**SoftwareInventoryLogging**資料庫中，展開**資料表**，以滑鼠右鍵按一下**HostInfo**資料表，然後選取前 1000 個資料列。 

    如果沒有資料的資料表中的一或多個主機，然後輪詢該/those 主機是否已順利完成至少一次。

 **檢查有從 Vm 或獨立伺服器，透過 HTTPS 推送資料的資料：** 

1. 開啟**SSMS** ，並連接到**Database Engine**。
a2. 依序展開**資料庫**，展開**SoftwareInventoryLogging**資料庫中，展開**資料表**，以滑鼠右鍵按一下**VMInfo**資料表，然後選取前 1000 個資料列。 

    >[!NOTE] 
    >唯一的虛擬機器的每個資料列會代表其中一個處理**bmil**檔案成功透過 HTTPS 推送，並由 SIL 彙總工具處理。 Bmil 檔案 SIL 所使用的專屬檔案，將會建立每個我們依每個 SIL 執行個體，這是只有必要時請注意 SIL 和 SILA 適用於虛擬環境中。 否則只有 HTTPS 流量的必要/預期的結果）。

 之後處理 cube 時，資料庫中的所有資料應該會都反映在 SIL 報告。

###<a name="data-flow-issue-2"></a>資料流問題 2
####<a name="too-many-servers-under-unknown-host"></a>未知的主機 下方的太多伺服器

這是可能發生在虛擬環境中，當 SIL 彙總工具未成功輪詢要用來裝載虛擬機器的實體主機。

1. 開啟**PowerShell**系統管理員身分執行**get silvmhost** cmdlet。

    -   如果為列出的主機**未知**，則**新增 silvmhost** cmdlet 未正確運作-通常是因為存取加入這些主機的認證錯誤 (因此，無法辨識)。 如果認證經過驗證，這可能表示自動偵測，但是**hosttype**並**hypervisortype**中**新增 silvmhost** cmdlet 無法辨識平台。 那里一些進階的疑難排解步驟適用於這些情況下，但這裡未涵蓋 （請檢查 EventViewer SIL 彙總工具通道）。

    -   如果列出的主機，並**hosttype**並**hypervisortype**值不會列出**未知**，也就是 Windows 和 HyperV、 或 Ubuntu 和 Xen 等，但沒有沒有**datetime**下方**最近輪詢**資料行，則輪詢未成功尚未開始。

        -   您必須等候一小時之後新增輪詢的主機進行 (假設這個間隔設定為預設值-可以使用來檢查**get silaggregator** cmdlet)。

        -   如果已新增主機，因為過去一小時的時間，請檢查輪詢工作正在執行：在 **工作排程器**，選取**Software Inventory Logging Aggregator**之下**Microsoft** &gt; **Windows**並檢查工作歷程記錄。

    -   如果主機列出，但是沒有值，如**RecentPoll**， **HostType**，或**HypervisorType**，這主要是忽略。 這才會發生在 HyperV 環境中。 此資料實際上是來自 Windows Server VM，用來識別它在 HTTPS 上執行的實體主機。 這可用於識別報告，但需要採礦資料庫使用的特定 VM **Get SilAggregatorData** cmdlet。

之後會正確地輪詢的主機，您將能夠看到這些 SILA 資料庫中的實體主機的資料沒有**datetime**下方**最近輪詢**。 問題 1 節會提供步驟，來擷取這項資料。

###<a name="data-flow-issue-3"></a>資料流問題 3
####<a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>太多的實體主機，列為 未知的 OS 的 vm

1. 挑選一個 Windows Server 終端節點 (VM) 您知道是其中一個這些主機，以系統管理員身分登入。
2. 系統管理員身分開啟 PowerShell。
3. 請確認**SilLogging**正在使用**Get-sillogging** cmdlet。
 - 如果執行時，嘗試以手動方式使用，以將推送 SIL 資料**Publish-sildata**。

  - 如果沒有發生錯誤：
     - 請確定**targeturi**已**https://** 項目中。
     - 請確定已符合所有必要條件 
     - 請確定已安裝所有必要的更新，適用於 Windows Server （請參閱 SIL 的必要條件）。 檢查 (WS 2012 R2 上僅） 的快速方式是尋找這些使用下列 cmdlet:**Get-SilWindowsUpdate \*3060, \*3000**
     - 請確定用於向彙總工具的憑證安裝在要清查與在本機伺服器上正確的存放區**SilLogging**。
     - 在 SIL 彙總工具中，務必要用來進行彙總工具驗證憑證的憑證指紋加入至清單中使用**Set-silaggregator** **-AddCertificateThumbprint**cmdlet。
     - 如果使用企業憑證，請檢查 SIL 啟用的伺服器已加入建立憑證時所針對的網域，或可由根授權單位來驗證。 如果嘗試將資料轉送/推入彙總工具在本機電腦不信任憑證，這個動作會失敗並產生錯誤。

    如果上述所有已檢查並驗證，但問題持續發生：

    - 請檢查用來安裝 SIL 彙總工具的憑證狀況良好，且符合 SIL 彙總工具伺服器本身的名稱。 此外，如果使用企業憑證來安裝 SIL 彙總工具，則彙總工具可能需要加入該憑證建立所在的網域之中 (如果其他電腦順利轉送至相同的 SIL 彙總工具，則可以略過這些步驟)。

    -  最後，您可以檢查快取的 SIL 檔案嘗試轉送/推，在伺服器上的下列位置**\Windows\System32\Logfiles\SIL**。 如果**SilLogging**已啟動，而且已經執行超過一小時，或**Publish-sildata**才剛執行不久，並沒有任何檔案在此目錄中，比已彙總工具的記錄成功。

如果沒有任何錯誤，並在主控台上的任何輸出，然後資料推播/從發行 Windows Server 的 [結束] 節點以透過 HTTPS 的 SIL 彙總工具已成功。 若要依照系統管理員身分的 SIL 彙總工具的資料轉送，登入路徑，並檢查已到達資料檔案。 移至**Program Files (x86)** &gt; **Microsoft SIL 彙總工具** &gt; SILA 目錄。 您可以觀看即時抵達的資料檔案。

>[!NOTE] 
>一個以上的資料檔案可能已使用轉移**Publish-sildata** cmdlet。 SIL 上之結束節點會快取失敗的推播，最多 30 天。 在下一步 成功推送資料的所有檔案會都移至進行處理的彙總工具。 如此一來，新設定 SIL 彙總工具無法顯示資料在那之前它自己的安裝程式結束節點。

>[!NOTE] 
>目前沒有 SILA 遵循處理 SILA 目錄中只會在低流量情況下相關的資料檔案時的規則。 高流量一律會觸發即時處理。 預設行為是處理在 100 個檔案會在目錄中，或在 15 分鐘之後到達之後，請將 」 開始。 疑難排解端對端在小型環境中，時，通常必須等候 15 分鐘的時間。

處理這些檔案之後，您會看到資料庫中的資料。
