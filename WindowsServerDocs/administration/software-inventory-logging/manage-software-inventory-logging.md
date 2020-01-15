---
title: 管理軟體清查記錄
description: 說明如何管理軟體清查記錄
ms.custom: na
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 812173d1-2904-42f4-a9e2-de19effec201
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a14233e01c19df650d1059e1b60cd5398b05709a
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946998"
---
# <a name="manage-software-inventory-logging"></a>管理軟體清查記錄

>適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

本檔說明如何管理軟體清查記錄，這項功能可協助資料中心系統管理員在一段時間內，輕鬆地記錄其部署的 Microsoft 軟體資產管理資料。 本文件說明如何管理軟體清查記錄。 在使用 Windows Server 2012 R2 的軟體清查記錄之前，請確定每個需要清查的系統上都已安裝 Windows Update [kb 3000850](https://support.microsoft.com/kb/3000850)和[KB 3060681](https://support.microsoft.com/kb/3060681) 。 Windows Server 2016 不需要任何 Wndows 更新。 這項功能會在要清查的每部伺服器上本機執行。 它不會從遠端伺服器收集資料。  

軟體清查記錄功能也可以新增至 Windows Server 2012 R2 之前的兩個 Windows Server 版本。 您可以安裝下列更新，將軟體清查記錄功能新增至 Windows Server 2012 和 Windows Server 2008 R2 SP1：

- **Windows Server 2012 （Standard 或 Datacenter Edition）** 

> [!NOTE] 
> 套用下方的更新程式封裝前，請確定您已安裝了 [WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855)。

-  Windows Server 2012 的 WMF 4.0 更新程式封裝： [KB 3119938](https://support.microsoft.com/kb/3119938)

- **Windows Server 2008 R2 SP1**

> [!NOTE] 
> 套用下方的更新程式封裝前，請確定您已安裝了 [WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855)。


- 需要 [.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)


- Windows Server 2008 R2 的 WMF 4.0 更新程式封裝： [KB 3109118](https://support.microsoft.com/kb/3109118)


使用這項功能進行清查有兩種主要的方法：  
  
1.  啟動 SIL 記錄功能開始收集 SIL 資料來源，並透過網路每小時將裝載轉送到指定目標 (URI)。  
  
2.  使用 PowerShell 或 WMI 每隔一段時間手動查詢 SIL 資料。  
  
啟動 SIL 記錄需要一些規劃和深謀遠慮，但相較於手動查詢資料，它提供了顯著的優勢。 對資料中心系統管理員而言，使用 SIL 記錄有下列三個主要優勢：  
  
-   可以長時間收集持續的歷程記錄 (記錄)，並允許來自單一來源的富彈性且內容完整的報告。  
  
-   可以克服許多清查工具特有的電腦探索挑戰。  
  
-   因為資料已透過使用 SSL 的 HTTPS 進行加密，因此在克服許多清查工具特有之信任邊界挑戰和提高使用者權限需求的同時，還可以維持一定程度的安全性。  
  
依預設會安裝軟體清查記錄，但依預設並不會啟動記錄。 軟體清查記錄會透過 PowerShell Cmdlet 完成所有設定。 軟體清查記錄只有幾個設定選項。 本文件將說明這些選項及其預期用途，以及用於資料收集的 Cmdlet (如果使用上述的第二種方法)。  
  
**本檔中的**  
  
本文件涵蓋的設定選項包括：  
  
-   [啟動和停止軟體清查記錄](manage-software-inventory-logging.md#BKMK_Step1)  
  
-   [經過一段時間的軟體清查記錄](manage-software-inventory-logging.md#BKMK_Step2)  
  
-   [顯示軟體清查記錄資料](manage-software-inventory-logging.md#BKMK_Step3)  
  
-   [刪除軟體清查記錄所記錄的資料](manage-software-inventory-logging.md#BKMK_Step4)  
  
-   [備份及還原軟體清查記錄所記錄的資料] 管理-軟體-清查-記錄 md # BKMK_Step5）  
  
-   [讀取軟體清查記錄所記錄和發行的資料](manage-software-inventory-logging.md#BKMK_Step6)  
  
-   [軟體清查記錄安全性](manage-software-inventory-logging.md#BKMK_Step7)  
  
-   [在 Windows Server 軟體清查記錄中處理日期和時間設定](manage-software-inventory-logging.md#BKMK_Step8)  
  
-   [在掛接的虛擬硬碟中啟用和設定軟體清查記錄](manage-software-inventory-logging.md#BKMK_Step10)  
  
-   [在不含 KB 3000850 的 Windows Server 2012 R2 中使用軟體清查記錄的總覽](manage-software-inventory-logging.md#BKMK_Step11)  
  
-   [在 Windows Server 2012 R2 Hyper-v 環境中使用軟體清查記錄（不含 KB 3000850）](manage-software-inventory-logging.md#BKMK_Step12)  
  
> [!NOTE]  
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱使用指令程式。

  
## <a name="BKMK_Step1"></a>啟動和停止軟體清查記錄  
在執行 Windows Server 2012 R2 的電腦上必須啟用軟體清查記錄每日收集和透過網路轉送，才能記錄軟體清查。  
  
> [!NOTE]  
> 您可以使用 **[Get-SilLogging](https://technet.microsoft.com/library/dn283396.aspx)** PowerShell Cmdlet 來擷取軟體清查記錄服務的相關資訊，包括服務是執行中還是已停止等資訊。  
  
#### <a name="to-start-software-inventory-logging"></a>啟動軟體清查記錄  
  
1.  使用具有本機系統管理員權限的帳戶登入伺服器。  
  
2.  以系統管理員身分開啟 PowerShell。  
  
3.  在 PowerShell 命令提示字元中，輸入 **[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx)**  
  
> [!NOTE]  
> 在無需設定憑證指紋的情況下設定目標是可行的，但是如果您這樣做，轉送便會失敗，而且資料會被儲存在本機高達預設值 30 天 (之後便會被刪除)。 為目標設定了有效的憑證雜湊 (以及在本機電腦/個人存放區安裝了對應的有效憑證) 之後，只要將目標設定為接受設有此憑證的資料，便會將儲存在本機上的資料轉寄至目標 (如需詳細資訊，請參閱 [Software Inventory Logging Aggregator](Software-Inventory-Logging-Aggregator.md) )。  
  
#### <a name="to-stop-software-inventory-logging"></a>停止軟體清查記錄  
  
1.  使用具有本機系統管理員權限的帳戶登入伺服器。  
  
2.  以系統管理員身分開啟 PowerShell。  
  
3.  在 PowerShell 命令提示字元中，輸入 **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)**  
  
## <a name="configuring-software-inventory-logging"></a>設定軟體清查記錄  
設定軟體清查記錄將隨著時間推移所產生的資料轉寄至彙總伺服器，須執行下列三個步驟：  
  
1.  使用**start-sillogging – TargetUri**指定匯總伺服器的 web 位址（必須以 "HTTPs://" 開頭）。  
  
2.  使用 **Set-SilLogging –CertificateThumbprint** 指定有效 SSL 憑證的憑證指紋雜湊，其會用於驗證要傳輸到彙總伺服器的資料 (彙總伺服器必須設定為接受此雜湊)。  
  
3.  在要轉寄資料之來源本機伺服器的 **本機電腦/個人存放區** (或 **/LocalMachine/MY**) 安裝有效的 SSL 憑證。  
  
建議您先完成這些步驟，然後再使用 **Start-SilLogging**。  若要在使用 **Start-SilLogging**之後再執行這些步驟，只須停止再重新啟動 SIL 即可。  您也可以使用 Publish-SilData Cmdlet，確認彙總伺服器具有此伺服器的所有資料。  
  
如需設定整個 SIL framework 的完整指南，請參閱 [Software Inventory Logging Aggregator](software-inventory-logging-aggregator.md)。  特別是當 **Publish-SilData** 產生錯誤或 SIL 記錄失敗時，請參閱疑難排解一節。  
  
## <a name="BKMK_Step2"></a>經過一段時間的軟體清查記錄  
如果軟體清查記錄是由系統管理員啟動，系統便會開始進行每小時的收集和將資料轉送到彙總伺服器 (目標 URI)。 第一次轉送的資料會是與 [Get-SilData](https://technet.microsoft.com/library/dn283388.aspx) 在時間點擷取並顯示於主控台時相同的完整資料集。 此後，SIL 會每隔一段時間檢查資料，如果自上次收集後資料沒有變更，則只會將小型識別通知轉送至目標彙總伺服器。 如果已變更任何值，則 SIL 會再次傳送完整的資料集。  
  
> [!IMPORTANT]  
> 如果在隔一段時間後，無法存取目標 URI 或因為任何理由而無法透過網路進行資料傳輸，則收集的資料會被儲存在本機高達預設值 30 天 (這個時間之後便會被刪除)。 下一次成功轉送資料至目標彙總伺服器時，將會轉送所有儲存在本機的資料，並刪除本機快取資料。  
  
## <a name="BKMK_Step3"></a>顯示軟體清查記錄資料  
除了先前章節所述的 PowerShell Cmdlet 之外，還有其他 6 個 Cmdlet 可以用來收集軟體清查記錄資料：  
  
-   **[Get-silcomputer](https://technet.microsoft.com/library/dn283392.aspx)** ：顯示特定伺服器和作業系統相關資料的時間點值，以及實體主機的 FQDN 或主機名稱（如果有的話）。  
  
-   **[Get-silcomputeridentity （KB 3000850）](https://technet.microsoft.com/library/dn858074.aspx)** ：顯示 SIL 用於個別伺服器的識別碼。  
  
-   **[Publish-sildata](https://technet.microsoft.com/library/dn283388.aspx)** ：顯示所有軟體清查記錄資料的時間點集合。  
  
-   **[SilSoftware](https://technet.microsoft.com/library/dn283397.aspx)** ：顯示安裝在電腦上之所有軟體的時間點識別。  
  
-   **[Get-silualaccess](https://technet.microsoft.com/library/dn283389.aspx)** ：顯示唯一用戶端裝置要求的總數，以及伺服器在過去兩天內的用戶端使用者要求。  
  
-   **[Get-silwindowsupdate](https://technet.microsoft.com/library/dn283393.aspx)** ：顯示安裝在電腦上的所有 Windows 更新的時間點清單。  
  
軟體清查記錄 Cmdlet 的典型使用案例是系統管理員使用 [Get SilSoftware](https://technet.microsoft.com/library/dn283397.aspx)查詢軟體清查記錄，以取得所有軟體清查記錄資料的時間點集合。  
  
**輸出範例**  
  
```  
PS C:\> Get-SilData   
  
ID                 : 961FF8A1-8549-4BEC-8DF6-3B3E32C26FFA  
UUID               : B49ACB4C-7D9C-4806-9917-AE750BB3DA84  
VMGUID             : E84CCCBD-0D0F-486B-A424-9780C7CF92E4  
Name               : Server01Guest.Test.Contoso.com  
HypervisorHostName : Server01.Test.Contoso.com  
  
ID          : {F0C3E5D1-1ADE-321E-8167-68EF0DE699A5}  
Name        : Microsoft Visual C++ 2010  x86 Redistributable - 10.0.40219  
InstallDate : 12/5/2013  
Publisher   : Microsoft Corporation  
Version     : 10.0.40219  
  
ID          : {89F4137D-6C26-4A84-BDB8-2E5A4BB71E00}  
Name        : Microsoft Silverlight  
InstallDate : 3/20/2014  
Publisher   : Microsoft Corporation  
Version     : 5.1.30214.0  
  
ChassisSerialNumber       : 4452-0564-0284-2290-0113-6804-05  
CollectedDateTime         : 10/27/2014 4:01:33 PM  
Model                     : Virtual Machine  
Name                      : Server01Guest.Test.Contoso.com  
NumberOfCores             : 1  
NumberOfLogicalProcessors : 1  
NumberOfProcessors        : 1  
OSName                    : Microsoft Windows Server 2012 R2 Datacenter  
OSSku                     : 8  
OSSuite                   : 400  
OSSuiteMask               : 400  
OSVersion                 : 6.3.9600  
ProcessorFamily           : 179  
ProcessorManufacturer     : GenuineIntel  
ProcessorName             : Intel(R) Xeon(R) CPU           E5440  @ 2.83GHz  
SystemManufacturer        : Microsoft Corporation  
  
```  
  
> [!NOTE]  
> 此 Cmdlet 的輸出會與此功能的所有其他 **Get-Sil** Cmdlet 合併的結果相同，但會以非同步方式提供給主控台，因此物件的順序並不一定總是相同。  
>   
> 不一定要使用 **Get Sil** Cmdlet 才能啟動軟體清查記錄。  
  
## <a name="BKMK_Step4"></a>刪除軟體清查記錄所記錄的資料  
軟體清查記錄並不是做為關鍵元件使用。 它的設計目的是為了在維護高度可靠性的同時，儘可能減少對本機系統作業的影響。 這也可讓系統管理員手動刪除軟體清查記錄資料庫和支援檔案（\Windows\System32\LogFiles\SIL 目錄中的每個檔案），以符合操作需求。  
  
#### <a name="to-delete-data-logged-by-software-inventory-logging"></a>刪除軟體清查記錄所記錄的資料  
  
1. 在 PowerShell 中，使用 **[Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)** 命令停止軟體清查記錄。  
  
2. 開啟 [Windows 檔案總管]。  
  
3. 前往 **\Windows\System32\Logfiles\SIL\\**  
  
4. 刪除資料夾中的所有檔案。  
  
## <a name="BKMK_Step5"></a>備份和還原軟體清查記錄所記錄的資料  
如果透過網路轉送失敗，軟體清查記錄會暫時儲存每小時收集的資料。 記錄檔會儲存在 \Windows\System32\LogFiles\SIL\ 目錄中。 您可以搭配已排定的定期伺服器備份來進行此軟體清查記錄資料的備份。  
  
> [!IMPORTANT]  
> 如果作業系統因故必須進行修復安裝或升級，則儲存在本機的任何記錄檔將會遺失。  如果這是作業的重要資料，建議在新的作業系統安裝之前進行備份。 修復或升級之後，只需還原至相同的位置。  
  
> [!NOTE]  
> 如果因任何原因而導致 SIL 在本機登入的資料保留持續時間變得很重要，可以藉由變更這裡的登錄值來設定： \ HKEY_LOCAL_MACHINE\\SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging。 預設值為 ' 30 ' 30 天。  
  
## <a name="BKMK_Step6"></a>讀取軟體清查記錄所記錄和發行的資料  
SIL 記錄但儲存在本機的資料（如果轉送到目標 URI 失敗），或成功轉送到目標匯總伺服器的資料，會儲存在二進位檔案中（適用于每日的資料）。 若要在 PowerShell 中顯示這項資料，請使用 [Import-BinaryMiLog](https://technet.microsoft.com/library/dn262592.aspx) Cmdlet。  
  
## <a name="BKMK_Step7"></a>軟體清查記錄安全性  
若要順利地從軟體清查記錄 WMI 與 PowerShell API 中擷取資料，您必須要有本機伺服器上的系統管理權限。  
  
若要成功運用軟體清查記錄功能的完整功能，並在一段時間內持續 (以每個小時為間隔) 將資料轉送至彙總點，則系統管理員必須採用用戶端憑證，以確保安全的 SSL 工作階段以供透過 HTTPS 傳輸資料使用。 您可以在這裡找到 HTTPS 驗證的基本概觀： [HTTPS 驗證](https://technet.microsoft.com/library/cc736680(v=WS.10).aspx)。  
  
只有本機伺服器上的系統管理權限，才能存取在 Windows Server 上本機儲存的任何資料 (只有當已啟動此功能，但因故無法存取目標時才會發生這個情況)。  
  
## <a name="BKMK_Step8"></a>使用 Windows Server 2012 R2 軟體清查記錄中的日期和時間設定  
  
-   使用 [Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TimeOfDay 來設定執行 SIL 記錄的時間時，您必須指定日期和時間。 將設定行事曆日期，而且在到達日期之前（以本機系統時間為限），將不會進行記錄。  
  
-   使用[SilSoftware](https://technet.microsoft.com/library/dn283397.aspx)或[get-silwindowsupdate](https://technet.microsoft.com/library/dn283393.aspx)時，"InstallDate" 一律會顯示12：00：上午 10:00 pst，這是無意義的值。  
  
-   使用[get-silualaccess](https://technet.microsoft.com/library/dn283389.aspx)時，"SampleDate" 一律會顯示11：59：00，這是無意義的值。  日期是這些 Cmdlet 查詢的相關資料。  
  
## <a name="BKMK_Step10"></a>在掛接的虛擬硬碟中啟用和設定軟體清查記錄  
離線虛擬機器也支援軟體清查記錄的設定及啟用。 這適用于這種情況的實際用途，是為了涵蓋跨資料中心進行寬部署的「黃金影像」安裝，以及設定從部署到雲端部署的終端使用者映射。  
  
若要支援這些用途，軟體清查記錄會有與每個可設定選項相關聯的登錄項目。  您可以在 \ HKEY_LOCAL_MACHINE\\SOFTWARE\Microsoft\Windows\SoftwareInventoryLogging. 中找到這些登錄值  
  
|||||  
|-|-|-|-|  
|**Function**|**值名稱**|**資料**|**對應的 Cmdlet （僅適用于執行中的 OS）**|  
|啟動/停止功能|CollectionState|1 或 0|[Start-SilLogging](https://technet.microsoft.com/library/dn283391.aspx)、 [Stop-SilLogging](https://technet.microsoft.com/library/dn283394.aspx)|  
|在網路上指定目標彙總點|TargetUri|字串|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TargetURI|  
|指定用於目標 Web 伺服器 SSL 驗證的憑證指紋或憑證雜湊|憑證指紋|字串|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -CertificateThumbprint|  
|指定應該開始功能的日期和時間 (如果值設定在未來，以本機系統時間為準)|CollectionTime|預設：2000-01-01T03:00:00|[Set-SilLogging](https://technet.microsoft.com/library/dn283387.aspx) -TimeOfDay|  
  
若要在離線 VHD 上修改這些值 (未執行 VM OS)，則 VHD 必須先掛接，然後才可以使用下列命令來進行變更：  
  
-   [Reg load](https://technet.microsoft.com/library/cc742053.aspx)  
  
-   [Reg delete](https://technet.microsoft.com/library/cc742145.aspx)  
  
-   [Reg add](https://technet.microsoft.com/library/cc742162.aspx)  
  
-   [Reg unload](https://technet.microsoft.com/library/cc742043.aspx)  
  
啟動作業系統時，軟體清查記錄會檢查這些值並照著執行。  
  
## <a name="BKMK_Step11"></a>在不含 KB 3000850 的 Windows Server 2012 R2 中使用軟體清查記錄的總覽  
下列是根據 [KB 3000850](https://support.microsoft.com/kb/3000850)所做的軟體清查記錄功能和預設設定的變更：  
  
-   啟動 SIL 記錄時，透過網路收集和轉送的預設間隔會從每天變更為每小時 (每個小時內隨機進行)。  
  
-   預設資料裝載會簡化成只包括從 Get-SilComputer、Get-SilComputerIdentity 和 Get-SilSoftware 的物件。  
  
-   已移除 HYPER-V 環境中客體到主機的通道通訊。  
  
## <a name="BKMK_Step12"></a>在 Windows Server 2012 R2 Hyper-v 環境中使用軟體清查記錄（不含 KB 3000850）  
  
> [!NOTE]  
> 在 [KB 3000850](https://support.microsoft.com/kb/3000850) 更新的安裝中已移除這項功能。  
  
在 Windows Server 2012 R2 Hyper-v 主機上使用軟體清查記錄時，如果已在來賓中啟動 SIL 記錄，則可以從本機執行的 Windows Server 2012 R2 來賓中取出 SIL 資料。 不過，只有在使用 Publish-sildata 和 Publish-sildata Powershell Cmdlet 時，才有可能發生這種情況，而且只有在主機和來賓的 WIndows Server 2012 R2 中才可行。  這項功能的目的是讓提供來賓 Vm 的資料中心系統管理員，或大型公司的其他實體，在虛擬機器主機上捕獲軟體清查資料，然後再將所有此資料轉送到匯總工具（或目標 URI）。  
  
以下兩個範例說明 PowerShell 主控台上的輸出會在 Windows Server 2012 R2 Hyper-v 主機上執行，其中包含 SIL 記錄啟動的 windows server 2012 R2 的來賓 VM。  您會注意到第一個使用 Publish-sildata 的範例，將會如預期般輸出主機中的所有資料。  此外，也包含來賓的所有 SIL 資料，但會以折迭的格式呈現。  若要從來賓擴充及查看這項資料，只需剪下並貼上下列第二個範例中所使用的程式碼片段。  來自來賓的 SIL 資料物件一律會有物件中相關聯的 VM GUID。  
  
> [!NOTE]  
> 由於 SIL 資料會輸出在主控台上，使用 Get-SilData Cmdlet 時，在資料流中，不一定永遠都能以預測的順序輸出物件。  在下列兩個範例中，文字已使用色彩標示（藍色代表實體主機資料，而綠色代表虛擬來賓資料），僅做為此檔的說明工具。  
  
**輸出範例1**  
  
![](../media/software-inventory-logging/SILHyper-VExample1.png)  
  
**輸出範例 2** （w/Expand-publish-sildata 函數）  
  
![](../media/software-inventory-logging/SILHyper-VExample2.png)  
  
## <a name="see-also"></a>請參閱  
[軟體清查記錄入門](get-started-with-software-inventory-logging.md)  
[軟體清查記錄彙總工具](software-inventory-logging-aggregator.md)  
[Windows PowerShell 中的軟體清查記錄 Cmdlet](https://technet.microsoft.com/library/dn283390.aspx)  
[匯入-Import-binarymilog](https://technet.microsoft.com/library/dn262592.aspx)  
[匯出-Import-binarymilog](https://technet.microsoft.com/library/dn262591.aspx)  
  

