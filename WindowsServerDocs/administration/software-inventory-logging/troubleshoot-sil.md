---
title: 對軟體清查記錄進行疑難排解
description: 說明如何解決常見的軟體清查記錄部署問題。
ms.topic: article
author: brentfor
ms.author: brentf
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 71a6c4a04a0a9da45d020f71b3bb0685cd5c0c59
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628174"
---
# <a name="troubleshoot-software-inventory-logging"></a>對軟體清查記錄進行疑難排解

>適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

## <a name="understanding-sil"></a>瞭解 SIL

開始針對 SIL 進行疑難排解之前，您應該先充分瞭解其元件及其運作方式。 下列影片概述 SIL 和 SIL 匯總工具，以及如何使用它們來轉送和報告清查資料：

1. [軟體清查記錄簡介 (SIL)  (10:57) ](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [軟體清查記錄：設定 SIL 匯總工具 (14:34) ](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [軟體清查記錄：啟用 SIL 轉送 (7:20) ](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

## <a name="how-sil-data-flow-works"></a>SIL 資料流程的運作方式

SIL 架構有兩個主要元件和兩個通訊通道。 成功部署 SIL 時，這兩個通道以及兩個元件之間的資料流程都是必要的， (這會假設虛擬化或雲端環境-純粹的實體環境只需要) 的其中一個通道。 您必須瞭解 SIL 的元件和資料流程，才能正確部署。 觀賞上述的總覽影片之後，您會看到架構圖表，其中說明元件和這兩個通道的資料流程。 橙色箭號表示透過 WinRM 進行遠端查詢，綠色虛線箭號表示從每個 WS end 節點 SIL 的 SIL 匯總工具的 HTTPS 文章：

![SIL 架構圖表](../media/software-inventory-logging/image1.png)

如果您遇到 SIL 的問題，可能是因為通道的資料流程中斷，以及元件之間的中斷。 以下是與資料流程相關的最常見問題，請遵循下一節中的疑難排解步驟來解決這三個問題：

-   **資料流程問題 1** – **使用 SilReport 指令 (時，報表中沒有資料** ，或通常會遺失資料) 。

-   **資料流程問題 2** -報表中 **有未知主控制項的伺服器太多** 。

-   **資料流程問題 3** -在執行 SIL 的 Windows 伺服器上， **實體主機下的 vm 數目太多** ，在報告中列為不明 OS，以及/或在使用 **get-sildata** 時產生錯誤。

## <a name="troubleshooting-data-flow-issues"></a>針對資料流程問題進行疑難排解

開始之前，您必須先知道 SIL 匯總工具開始使用 **SilAggregator** Cmdlet 的時間長度。

>[!IMPORTANT]
>在3AM 本機系統時間處理 SQL 資料 cube 之前，報表中不會有任何資料。 在 cube 處理資料之前，請勿繼續進行疑難排解步驟。

如果您要針對報表中的資料進行疑難排解 (或遺漏報表) 比上次處理 cube 的時間還新，或是在 cube 之前處理新安裝) 的 (之前，請遵循下列步驟來即時處理 SQL 資料 cube：

1. 以 SQL Server 的系統管理員身分登入，並在命令提示字元中執行 **SSMS** 。
2. 連線至資料庫引擎。
3. 展開 **SQL Server Agent** 然後展開 [ **作業**]。
4. 在 [ **SILStagingRefresh** ] 上按一下滑鼠右鍵，然後選取 [ **在步驟啟動作業**]。
5. 按一下 [ **開始** ]，然後等候重新整理進度列完成。
6. 以系統管理員身分開啟 PowerShell，並執行 **silreport-openreport** Cmdlet。

如果報表中仍沒有任何資料，請繼續進行這三個數據流問題的疑難排解。

### <a name="data-flow-issue-1"></a>資料流程問題1

#### <a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>使用 SilReport 指令程式時，報表中沒有任何資料 (或資料通常會遺失) 

如果遺失資料，可能是因為 SQL 資料 cube 尚未處理。 如果最近已處理過，而且您認為遺漏的資料應該在 cube 處理之前抵達匯總工具，請依照反向順序來追蹤資料的路徑。 挑選唯一的主機和唯一的 VM 來進行疑難排解。 相反地，資料路徑會是 **SILA Report** &lt; **SILA database** &lt; **SILA 本機目錄** &lt; **遠端實體主機** ，或執行 **SIL agent/task 的 WS VM**。

#### <a name="check-to-see-if-data-is-in-the-database"></a>查看資料是否在資料庫中

有兩種方式可以檢查資料： **Powershell** 或 **SSMS**。

>[!Important]
>如果在 SILA 將資料插入資料庫之後，cube 至少已經處理過一次，則這項資料應該會反映在報表中。 如果資料庫中沒有任何資料，則輪詢實體主機會失敗，或未透過 HTTPS 或兩者來接收任何專案。

 #### <a name="powershell"></a>PowerShell

1. 以系統管理員身分開啟 PowerShell 並執行 **silvmhost** Cmdlet，然後執行 **silaggregator**。

    >[!NOTE]
    >**Silaggregator**的輸出一律會模仿 Excel 報表的 [ **Windows Server 詳細資料]** 索引標籤。 不預期會有不同的結果。

2. 執行 **get-silvmhost**
   - 如果未列出任何主機，請使用 **silvmhost** Cmdlet 來新增主機。
   - 如果主機列為 **未知**，請移至問題2。
   d-如果列出主機，但**最近的輪詢**資料行下沒有**日期時間**，請移至下方的**問題 2** 。

**其他相關的命令**

**SilAggregator- &lt; 推送資料 &gt; 的已知伺服器的 Computername fqdn**：這會從資料庫產生有關電腦 (VM) 的資訊，甚至是在 cube 處理之前。 因此，您可以使用這個指令程式來檢查資料庫中的資料，讓 Windows Server 在 3AM (的 cube 進程之前、之前或不使用的情況下，透過 HTTPS 推送 SIL 資料，或者，如果您未依照本節開頭所述的即時重新整理 cube) 。

**SilAggregator-VmHostName 已 &lt; 輪詢實體主機的 fqdn，其中在使用 SilVmHost &gt; 指令程式時，最新的輪詢資料行中會有一個值**：這會從資料庫產生關於實體主機的資訊，甚至是在處理 cube 之前。

#### <a name="ssms"></a>SSMS

n**檢查要輪詢的主機資料：**

1. 開啟 **SSMS** 並連接到 **資料庫引擎**。
2. 展開 [ **資料庫**]，展開 [ **SoftwareInventoryLogging** ] 資料庫，展開 [ **資料表]**，以滑鼠右鍵按一下 **HostInfo** 資料表，然後選取 [前1000個數據列]。

    如果資料表中有一或多部主機的資料，則輪詢該主機或這些主機 (的) 已成功完成一次以上。

   **檢查透過 HTTPS 推送資料的 Vm 或獨立伺服器的資料：**

3. 開啟 **SSMS** 並連接到 **資料庫引擎**。
   答. 展開 [ **資料庫**]，展開 [ **SoftwareInventoryLogging** ] 資料庫，展開 [ **資料表]**，以滑鼠右鍵按一下 [ **VMInfo** ] 資料表，然後選取前1000個數據列。

    >[!NOTE]
    >唯一 VM 的每個資料列都代表一個已處理的 **bmil** 檔案已成功推送至 HTTPS，並由 SIL 匯總工具處理。 Bmil 檔案是 SIL 所使用的專屬檔案，每個 SIL 實例都會建立一個檔案，請注意，只有在虛擬環境中使用 SIL 和 SILA 時才需要此檔案。 否則，) 只需要 HTTPS 流量。

   在處理 cube 之後，資料庫中的所有資料都應該反映在 SIL 報表中。

### <a name="data-flow-issue-2"></a>資料流程問題2
#### <a name="too-many-servers-under-unknown-host"></a>未知主機下的伺服器太多

當 SIL 匯總工具無法成功輪詢裝載虛擬機器的實體主機時，可能會在虛擬環境中發生此情況。

1. 以系統管理員身分開啟 **PowerShell** ，然後執行 **silvmhost** Cmdlet。

    -   如果主機列為 [ **未知**]， **silvmhost 指令程式** 就無法正確運作，通常是因為為這些主機的存取新增了不當的認證 (因此，未知的) 。 但是，如果認證經過驗證，則可能表示自動偵測 **hosttype** 和 **hypervisortype** 在 **silvmhost** Cmdlet 中無法辨識平臺。 在這些情況下，您可以使用先進的疑難排解步驟，但此處未涵蓋 (檢查 EventViewer SIL 匯總工具通道) 。

    -   如果列出主機，且 **hosttype** 和 **HYPERVISORTYPE** 是以不 **知道**的值列出，即 Windows 和 HyperV、Ubuntu 和 Xen 等，但**最近輪詢**資料行沒有任何**日期時間**，但輪詢尚未成功。

        -   新增要進行輪詢的主機之後，您將需要等候一小時的時間 (假設此間隔設定為預設值–可使用 silaggregator 指令 **程式**) 檢查。

        -   如果在新增主機之後已經過了一小時，請檢查輪詢工作是否正在執行：在**工作排程器**中，選取 [ **Microsoft** Windows] 下的 [**軟體清查記錄**匯總工具]，並檢查工作的歷程 &gt; **Windows**記錄。

    -   如果已列出主機，但 **RecentPoll**、 **HostType**或 **HypervisorType**沒有任何值，則可能會被忽略。 這只會發生在 HyperV 環境中。 這項資料實際上是來自 Windows Server VM，它會識別其正在透過 HTTPS 執行的實體主機。 這有助於識別正在報告的特定 VM，但需要使用 **SilAggregatorData** 指令程式來建立資料庫。

當主機正確地輪詢之後，您就可以在 [**最近輪詢**] 下有**日期時間**的 SILA 資料庫中，看到這些實體主機的資料。 上述的 [問題 1] 區段提供了抓取此資料的步驟。

### <a name="data-flow-issue-3"></a>資料流程問題3
#### <a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>將 Vm 列為未知 OS 的實體主機太多

1. 挑選一個 Windows Server 終端節點 (您知道在其中一個主機上的 VM) ，以系統管理員身分登入。
2. 以系統管理員身分啟動 PowerShell。
3. 使用**執行 start-sillogging**指令程式確認**執行 start-sillogging**正在執行。
   - 如果正在執行，請嘗試使用 **發行-get-sildata**手動推送 SIL 資料。

   - 如果發生錯誤：
     - 確定 **targeturi** 在專案中有 **HTTPs://** 。
     - 確定符合所有先決條件
     - 確定已安裝 Windows Server 的所有必要更新 (參閱 SIL) 的必要條件。 只在 WS 2012 R2 上檢查 (的快速方法是使用下列 Cmdlet 來尋找這些) ： **get-silwindowsupdate \* 3060、 \* 3000**
     - 請確定用來向匯總工具驗證的憑證已安裝在本機伺服器上正確的存放區中，以使用 **執行 start-sillogging**進行清查。
     - 在 SIL 匯總工具上，請務必使用 **SilAggregator** **– AddCertificateThumbprint** Cmdlet 將用來驗證匯總的憑證憑證指紋新增至清單。
     - 如果使用企業憑證，請檢查 SIL 啟用的伺服器已加入建立憑證時所針對的網域，或可由根授權單位來驗證。 如果嘗試將資料轉送/推入彙總工具在本機電腦不信任憑證，這個動作會失敗並產生錯誤。

     如果上述所有動作都已檢查並驗證，但問題仍然存在：

     - 檢查用來安裝 SIL 匯總工具的憑證是否狀況良好，並符合 SIL 匯總伺服器本身的名稱。 此外，如果使用企業憑證來安裝 SIL 彙總工具，則彙總工具可能需要加入該憑證建立所在的網域之中 (如果其他電腦順利轉送至相同的 SIL 彙總工具，則可以略過這些步驟)。

     -  最後，您可以在嘗試轉寄/推送、 **\Windows\System32\Logfiles\SIL**的伺服器上，檢查下列位置是否有快取的 SIL 檔。 如果 **執行 start-sillogging** 已啟動且已執行超過一小時，或最近已執行過 **get-sildata** ，而且此目錄中沒有任何檔案，則與匯總工具之間的記錄已成功。

如果沒有任何錯誤，且主控台沒有任何輸出，則會成功從 Windows Server 終端節點將資料推送/發佈至透過 HTTPS 的 SIL 匯總工具。 若要追蹤資料的路徑，請以系統管理員身分登入 SIL 匯總工具，然後檢查已到達的資料檔案 (s) 。 移至 **Program Files (x86) ** &gt; **Microsoft SIL**匯總工具 &gt; SILA 目錄。 您可以即時監看傳入的資料檔案。

>[!NOTE]
>**Get-sildata** Cmdlet 可能已傳送一個以上的資料檔案。 終端節點上的 SIL 會快取失敗的推播，最多30天。 在下一次成功推送時，所有資料檔案都會移至匯總工具進行處理。 如此一來，新設定的 SIL 匯總工具就可以在其本身的設定之前，先從終端節點顯示資料。

>[!NOTE]
>在 SILA 目錄中處理只與低流量情況相關的資料檔案時，會有下列規則 SILA。 高流量一律會即時觸發處理。 預設行為是，在100檔案抵達目錄後或15分鐘之後，就會開始處理。 當您在小型環境中端對端進行疑難排解時，通常需要等待15分鐘。

處理這些檔案之後，您會看到資料庫中的資料。
