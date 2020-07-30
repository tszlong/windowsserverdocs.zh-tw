---
title: 對軟體清查記錄進行疑難排解
description: 說明如何解決常見的軟體清查記錄部署問題。
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.topic: article
author: brentfor
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: f6ad4686ce5afb86ce60ec0313bd675f8a9a1f4a
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408797"
---
# <a name="troubleshoot-software-inventory-logging"></a>對軟體清查記錄進行疑難排解

>適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

## <a name="understanding-sil"></a>瞭解 SIL

開始針對 SIL 進行疑難排解之前，您應該先充分瞭解其元件及其運作方式。 下列影片提供 SIL 和 SIL 匯總工具的總覽，以及如何使用它們來轉送和報告清查資料：

1. [軟體清查記錄（SIL）簡介（10:57）](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [軟體清查記錄：設定 SIL 匯總工具（14:34）](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [軟體清查記錄：啟用 SIL 轉送（7:20）](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

## <a name="how-sil-data-flow-works"></a>SIL 資料流程的運作方式

SIL 架構有兩個主要元件和兩個通訊通道。 若要成功進行 SIL 部署（這會假設虛擬化或雲端的環境，純粹的實體環境只需要其中一個通道），這兩個通道之間和兩個元件之間的資料流程都是必要的。 您必須瞭解 SIL 的元件和資料流程，才能正確部署。 觀看上述的總覽影片之後，您將會看到架構圖，說明這兩個通道的元件和資料流程。 橙色箭號表示透過 WinRM 進行遠端查詢，綠色虛線箭號表示從每個 WS 端點節點的 SIL 對 SIL 匯總工具進行 HTTPS post：

![SIL framework 圖表](../media/software-inventory-logging/image1.png)

如果您遇到 SIL 的問題，可能是因為通道的資料流量中斷，以及元件之間的干擾。 以下是與資料流程相關的最常見問題，如下一節所述的疑難排解步驟解決這三個問題：

-   **資料流程問題 1** –使用 SilReport Cmdlet （或通常遺失資料）**時，報表中沒有任何資料**。

-   **資料流程問題 2** –報表中的 [**不明主機] 下有太多伺服器**。

-   **資料流程問題 3** –在執行 SIL 的 Windows 伺服器上，**實體主機下的 vm 太多，列**于報告中，以及（或）使用**發佈 publish-sildata**時所產生的錯誤。

## <a name="troubleshooting-data-flow-issues"></a>針對資料流程問題進行疑難排解

開始之前，您必須先瞭解多久前 SIL 匯總工具開始使用**set-silaggregator** Cmdlet。

>[!IMPORTANT]
>在 SQL 資料 cube 于3AM 本機系統時間處理之前，報表中將不會有任何資料。 在 cube 處理資料之前，請勿繼續進行疑難排解步驟。

如果您要針對報表中的資料進行疑難排解（或報表中遺漏），這比上次處理 cube 的時間還新，或在處理過 cube 之前（針對新的安裝），請遵循下列步驟來即時處理 SQL 資料 cube：

1. 以 SQL Server 的系統管理員身分登入，並在命令提示字元中執行**SSMS** 。
2. 連線至資料庫引擎。
3. 展開**SQL Server Agent** ，然後展開 [**作業**]。
4. 以滑鼠右鍵按一下 [ **SILStagingRefresh** ]，然後選取 [**在步驟啟動作業**]。
5. 按一下 [**開始**]，並等候重新整理進度列完成。
6. 以系統管理員身分開啟 PowerShell，然後執行**silreport-openreport** Cmdlet。

如果報表中仍然沒有任何資料，請繼續進行這三個數據流問題的疑難排解。

### <a name="data-flow-issue-1"></a>資料流程問題1

#### <a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>使用 SilReport Cmdlet 時，報表中沒有任何資料（或一般資料遺失）

如果資料遺失，可能是因為 SQL 資料 cube 尚未處理。 如果最近已處理過，而且您認為遺漏的資料應該在 cube 處理之前抵達匯總工具，請依照反向順序來追蹤資料的路徑。 挑選唯一的主機和唯一的 VM 來進行疑難排解。 相反地，資料路徑會是**SILA Report** &lt; **SILA database** &lt; **SILA 本機目錄** &lt; **遠端實體主機**或執行**SIL 代理程式/工作的 WS VM**。

#### <a name="check-to-see-if-data-is-in-the-database"></a>查看資料是否在資料庫中

有兩種方式可以檢查資料： **Powershell**或**SSMS**。

>[!Important]
>如果在 SILA 將資料插入資料庫之後，cube 已處理至少一次，則此資料應該會反映在報表中。 如果資料庫中沒有任何資料，則輪詢實體主機會失敗，或不會透過 HTTPS 或兩者來接收任何內容。

 #### <a name="powershell"></a>PowerShell

1. 以系統管理員身分開啟 PowerShell 並執行**add-silvmhost** Cmdlet，然後執行**set-silaggregator**。

    >[!NOTE]
    >**Set-silaggregator**的輸出一律會模仿 Excel 報表的 [ **Windows Server 詳細資料]** 索引標籤。 不預期會有不同的結果。

2. 執行**add-silvmhost**
   - 如果沒有列出任何主機，請使用**add-silvmhost** Cmdlet 新增主機。
   - 如果主機列為 [**不明**]，請移至 [問題 2]。
   d-如果主機已列出，但 [**最近的輪詢**] 資料行下沒有**datetime** ，則請移至下方的**問題 2** 。

**其他相關命令**

**Set-silaggregator- &lt; 推送資料 &gt; 之已知伺服器的 Computername fqdn**：這會從資料庫產生關於電腦（VM）的資訊，即使在 cube 處理之前亦然。 因此，您可以使用這個指令程式來檢查資料庫中的資料，以供 Windows Server 在3AM 的 cube 進程（如果您未依照本節開頭所述的即時重新整理 cube）中推送 SIL 資料。

**Set-silaggregator-VmHostName 已 &lt; 輪詢的實體主機的 fqdn，其中在 [最近的輪詢] 資料行中有一個值在使用 add-silvmhost Cmdlet &gt; 時**：這會從資料庫產生有關實體主機的資訊，甚至是在 cube 處理之前。

#### <a name="ssms"></a>SSMS

n**檢查要輪詢之主機的資料：**

1. 開啟**SSMS**並連接到**資料庫引擎**。
2. 依序展開 [**資料庫**]、[ **SoftwareInventoryLogging** ] 資料庫、[**資料表]**，以滑鼠右鍵按一下**HostInfo**資料表，然後選取 [前1000個數據列]。

    如果資料表中有一或多個主機的資料，則輪詢該/這些主機至少已成功一次。

   **檢查已透過 HTTPS 推送資料的 Vm 或獨立伺服器的資料：**

3. 開啟**SSMS**並連接到**資料庫引擎**。
   a2. 依序展開 [**資料庫**]、[ **SoftwareInventoryLogging** ] 資料庫和 [**資料表]**，以滑鼠右鍵按一下 [ **VMInfo** ] 資料表，然後選取前1000個數據列。

    >[!NOTE]
    >唯一 VM 的每個資料列都代表一個已處理的**bmil**檔案已成功推送至 HTTPS，並由 SIL 匯總程式處理。 Bmil 檔案是 SIL 所使用的專屬檔案，每個 SIL 實例都會建立一個檔案，請注意，只有在虛擬環境中使用 SIL 和 SILA 時才需要此檔案。 否則，只需要/預期 HTTPS 流量。

   在 cube 處理之後，資料庫中的所有資料都應該反映在 SIL 報表中。

### <a name="data-flow-issue-2"></a>資料流程問題2
#### <a name="too-many-servers-under-unknown-host"></a>不明主機下的伺服器太多

當 SIL 匯總工具無法順利輪詢裝載虛擬機器的實體主機時，這可能會發生在虛擬環境中。

1. 以系統管理員身分開啟**PowerShell** ，並執行**add-silvmhost** Cmdlet。

    -   如果主機列為 [**不明**]， **add-silvmhost 指令程式**無法正常運作–通常是因為新增了錯誤的認證來存取這些主機（因而不明）。 但是，如果驗證認證，這可能表示**add-silvmhost** Cmdlet 中的**hosttype**和**hypervisortype**自動偵測無法辨識平臺。 在這些情況下，您可以使用先進的疑難排解步驟，但此處並未涵蓋（請檢查 EventViewer SIL 匯總程式通道）。

    -   如果列出主機，而且**hosttype**和**hypervisortype**列出的值不是**未知**的，即 Windows 和 HyperV、Ubuntu 和 Xen 等等，但是 [**最近的輪詢**] 資料行下沒有 [**日期時間**]，但輪詢尚未成功發生。

        -   新增要進行輪詢的主機之後，您必須等候一小時（假設此間隔設定為預設值–可以使用**set-silaggregator** Cmdlet 來檢查）。

        -   如果自從新增主機之後已經過一小時，請檢查輪詢工作是否正在執行：在**工作排程器**中，選取 [ **Microsoft** Windows] 底下的 [**軟體清查記錄**匯總工具]， &gt; **Windows**然後檢查工作的歷程記錄。

    -   如果主機已列出，但沒有**RecentPoll**、 **HostType**或**HypervisorType**的值，這可能會被忽略。 這只會在 HyperV 環境中發生。 此資料實際上來自 Windows Server VM，可識別它透過 HTTPS 執行的實體主機。 這有助於識別已報告的特定 VM，但需要使用**SilAggregatorData 指令程式**來挖掘資料庫。

當主機正確輪詢之後，您就可以在 SILA 資料庫中看到這些實體主機的資料，其中包含**最近輪詢**的**日期時間**。 上述的「問題1」一節提供抓取此資料的步驟。

### <a name="data-flow-issue-3"></a>資料流程問題3
#### <a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>虛擬機器的實體主機過多，列為不明 OS

1. 挑選一個您知道在其中一個主機上的 Windows Server 終端節點（VM），以系統管理員身分登入。
2. 以系統管理員身分啟動 PowerShell。
3. 使用**start-sillogging**指令程式確認**start-sillogging**正在執行。
   - 如果正在執行，請嘗試使用**發行-publish-sildata**手動推送 SIL 資料。

   - 如果發生錯誤：
     - 確定**targeturi**已**HTTPs://** 在專案中。
     - 確保符合所有先決條件
     - 請確定已安裝 Windows Server 的所有必要更新（請參閱 SIL 的必要條件）。 若要快速檢查（僅限 WS 2012 R2），請使用下列 Cmdlet 來尋找這些方法： **get-silwindowsupdate \* 3060、 \* 3000**
     - 確保用來驗證匯總工具的憑證安裝在本機伺服器上要以**start-sillogging**清查的正確存放區中。
     - 在 SIL 匯總工具上，請務必使用**set-silaggregator** **– AddCertificateThumbprint** Cmdlet，將用來驗證匯總工具的憑證憑證指紋新增至清單。
     - 如果使用企業憑證，請檢查 SIL 啟用的伺服器已加入建立憑證時所針對的網域，或可由根授權單位來驗證。 如果嘗試將資料轉送/推入彙總工具在本機電腦不信任憑證，這個動作會失敗並產生錯誤。

     如果上述所有動作都已核取並驗證，但是問題仍然存在：

     - 檢查用來安裝 SIL 匯總工具的憑證是否狀況良好，以及是否符合 SIL 匯總伺服器本身的名稱。 此外，如果使用企業憑證來安裝 SIL 彙總工具，則彙總工具可能需要加入該憑證建立所在的網域之中 (如果其他電腦順利轉送至相同的 SIL 彙總工具，則可以略過這些步驟)。

     -  最後，您可以在嘗試轉送/推送、 **\Windows\System32\Logfiles\SIL**的伺服器上，檢查下列位置是否有快取的 SIL 檔案。 如果**start-sillogging**已啟動且執行超過一小時，或最近已執行過**publish-sildata** ，而且這個目錄中沒有任何檔案，則記錄到匯總工具已成功。

如果沒有任何錯誤，而且主控台上沒有任何輸出，則會成功從 Windows Server 終端節點將資料推送/發行至 SIL 匯總工具。 若要追蹤資料的路徑，請以系統管理員身分登入 SIL 匯總工具，並檢查已抵達的資料檔案。 移至**Program Files （x86）** &gt; **Microsoft SIL**匯總工具 &gt; SILA 目錄。 您可以即時監看傳入的資料檔案。

>[!NOTE]
>有一個以上的資料檔案可能已經使用**publish-sildata** Cmdlet 傳輸。 結束節點上的 SIL 會快取失敗的推送最多30天。 在下一次成功推送時，所有資料檔案都會移至匯總工具進行處理。 如此一來，新設定的 SIL 匯總工具就可以在其本身的設定之前，顯示來自結束節點的資料。

>[!NOTE]
>在處理 SILA 目錄中的資料檔案時，只有在低流量情況下才相關的規則 SILA。 高流量一律會即時觸發處理。 預設行為是，在100檔案抵達目錄後或15分鐘後，就會開始處理。 在小型環境中進行端對端疑難排解時，通常需要等候15分鐘。

處理這些檔案之後，您將會在資料庫中看到資料。
