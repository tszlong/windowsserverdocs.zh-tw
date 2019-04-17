---
title: 使用 Robocopy DFS 複寫 preseed 檔案
description: 如何使用 Robocopy.exe DFS 複寫 preseed 檔案。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7a14b1a1e0f91002b201869e4c68187ffaf3f8f
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081999"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>使用 Robocopy DFS 複寫 preseed 檔案

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

本主題說明如何使用命令列工具， **Robocopy.exe**，來設定 [分散式檔案系統 (DFS) 複寫 （也稱為 DFSR 或 DFS-R） Windows Server 中的複寫時 preseed 檔案。 由之前設定 DFS 複寫 preseeding 檔案、 新增新的複寫協力廠商或取代伺服器，您可加速初始同步處理並啟用 Windows Server 2012 R2 中 DFS 複寫資料庫藉由複製。 Robocopy 方法是其中一個數個 preseeding 方法 ；如需概觀，請參閱[步驟 1： DFS 複寫 preseed 檔案](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>)。

Robocopy （完善檔案複本） 的命令列公用程式隨附於 Windows Server。 公用程式提供廣泛的選項，包括複製安全性、 備份 API 支援、 重試功能及記錄。 更新的版本包含多執行緒及未緩衝的 I/O 支援。

>[!IMPORTANT]
>Robocopy 不會複製獨佔鎖定的檔案。 如果使用者通常會鎖定長期間檔案伺服器上的許多檔案，請考慮使用不同的 preseeding 方法。 Preseeding 不需要在來源和目的地伺服器上的檔案清單相符完美但不是存在於執行初始同步處理的 DFS 複寫、 低效率 preseeding 時的多個檔案。 若要降低鎖定衝突，使用 Robocopy 組織的非尖峰時間。 永遠檢查 Robocopy 記錄之後 preseeding 若要確認您已瞭解哪些檔案已略過因獨佔鎖定。

若要使用 Robocopy DFS 複寫 preseed 檔案，請遵循下列步驟：

1. [下載並安裝最新版的 Robocopy。](#step-1:-download-and-install-the-latest-version-of-robocopy)
2. [穩定將複製的檔案。](#step-2:-stabilize-files-that-will-be-replicated)
3. [將複製的檔案複製到目的地伺服器。](#step-3:-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>先決條件

Preseeding 不會不會直接包含 DFS 複寫，因為您只須符合執行使用 Robocopy 檔案複本的需求。

- 您必須是來源與目的地伺服器上的本機管理員群組成員的帳戶。

- 您要用以將檔案複製的伺服器上安裝最新版本的 Robocopy — 來源伺服器或目的地伺服器 ；您必須安裝最新版本的作業系統版本。 指示，請參閱[步驟 2： 穩定就會複製檔案](#step-2:-stabilize-files-that-will-be-replicated)。 除非您已 preseeding 檔案從執行 Windows Server 2003 R2 的伺服器，您可以執行 Robocopy 的來源或目的地伺服器上。 目的地伺服器，通常有較新版的作業系統版本，可讓您存取 Robocopy 的最新版本。

- 確定目的地的磁碟機上提供足夠的儲存空間。 不要在想要複製到路徑上建立資料夾： Robocopy 必須建立根資料夾。
    
    >[!NOTE]
    >當您決定要為 preseeded 檔案配置的空間時，請考慮預期的資料成長透過 DFS 複寫的時間及儲存需求。 規劃說明，請參閱[管理 DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>)複寫的 [[編輯的臨時資料夾和因衝突而刪除資料夾配額大小](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11))。

- 在來源伺服器上，選擇性地安裝程序監視器或程序總管] 中，可讓您用來檢查應用程式的已鎖定檔案。 下載 （英文） 資訊，請參閱[程序監視器](https://docs.microsoft.com/sysinternals/downloads/procmon)和[程序 Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer)。

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>步驟 1： 下載及安裝最新版的 Robocopy

使用 Robocopy preseed 檔案之前，您應該下載及安裝最新版的**Robocopy.exe**。 這可確保 DFS 複寫不會因為內 Robocopy 的傳送版本問題而略過檔案。

最新的相容 Robocopy 版本的來源伺服器執行的 Windows Server 的版本而定。 如需下載 Robocopy for Windows Server 2008 R2 或 Windows Server 2008 的最新版本與 hotfix 資訊，請參閱[分散式檔案系統 (DFS) 技術及 Windows Server 2008 中目前可用的 hotfix 清單Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t)。

或者，您可以找出並採取下列步驟安裝作業系統的最新 hotfix。

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>找出並安裝最新版的 Robocopy 特定版本的 Windows Server

1. 在網頁瀏覽器中開啟[https://support.microsoft.com](https://support.microsoft.com/)。

2. 在 [**搜尋支援**，輸入下列字串，並取代`<operating system version>`具有適當的作業系統中，然後按下 Enter 鍵：
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    例如，輸入**robocopy.exe kbqfe"Windows Server 2008 R2"**。

3. 找出並下載具有最高的編號 （也就是最新版本） 的 hotfix。

4. 在伺服器上安裝 hotfix。

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>步驟 2： 穩定將複製的檔案

在伺服器上安裝最新版的 Robocopy 之後，您應該防止鎖定的檔案封鎖複製使用下表所述的方法。 大部分的應用程式不會以獨佔模式鎖定檔案。 不過，正常作業期間可能會鎖定檔案的小型百分比檔案伺服器上。

|鎖定的內容來源|說明|防護功能|
|---|---|---|
|使用者從遠端開啟檔案共用上。|員工連線至標準檔案伺服器及編輯文件、 多媒體內容或其他檔案。 有時稱為傳統首頁資料夾或共用的資料工作量。|僅執行 Robocopy 作業期間離峰、 非上班時間。 這最小化 Robocopy 必須期間 preseeding 略過的檔案的數目。<br><br>考慮暫時使用 Windows PowerShell**授與 SmbShareAccess**和**關閉 SmbSession**指令程式，就會複製檔案共用上設定的唯讀權限。 如果您將一般群組，例如所有人或經過認證之使用者的權限以讀取、 標準很可能無法開啟檔案獨佔鎖定 （如果其應用程式偵測唯讀存取開啟檔案時）。<br><br>您也可能會考慮設定的暫存的防火牆規則 SMB 連接埠 445 輸入至封鎖的檔案的存取或使用**封鎖 SmbShareAccess**指令程式，該伺服器。 但是，這兩種方法是非常中斷使用者作業。|
|應用程式開啟本機檔案。|有時檔案伺服器上執行的應用程式工作量鎖定檔案。|暫時停用或解除安裝之應用程式的已鎖定檔案。 您可以使用處理序監視器] 或 [處理序檔案總管來決定哪些應用程式所鎖定檔案。 若要下載程序監視器或程序檔案總管]，瀏覽的[程序監視器](https://docs.microsoft.com/sysinternals/downloads/procmon)和[程序檔案總管](https://docs.microsoft.com/sysinternals/downloads/process-explorer)的頁面。|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>步驟 3： 將複製的檔案複製到目的地伺服器

減少鎖定，就會複製檔案之後，您可以 preseed 從來源伺服器的檔案至目的伺服器。

>[!NOTE]
>您可以執行 Robocopy 來源電腦或目的地電腦上。 下列程序說明執行 Robocopy 在目的地伺服器上，這通常執行較新版作業系統，若要利用較新版的作業系統可能會提供任何其他 Robocopy 功能。

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Preseed 複製到目的地伺服器具有 Robocopy 檔案

1. 目的地伺服器的來源和目的地伺服器上的本機管理員群組的成員帳戶登入。

2. 開啟提升權限的命令提示字元。

3. 若要 preseed 到目的地伺服器的檔案從來源，執行下列命令，worked 您自己的來源、 目的地及記錄檔路徑以方括弧括起的值：
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    此命令會將所有內容來源] 資料夾都複製到 [目的地] 資料夾中，使用下列參數：
    
    |參數|說明|
    |---|---|
    |"< 來源複製的資料夾路徑 > \"|指定要 preseed 目的地伺服器上的來源資料夾。|
    |"\ < 目的地複製的資料夾路徑 >"|指定要儲存 preseeded 的檔案的資料夾路徑。<br><br>在目標資料夾不必須已經存在之目的地伺服器上。 若要取得比對檔案雜湊，Robocopy 必須建立根資料夾時加以 preseeds 檔案。|
    |/e|會複製子目錄和其檔案，以及空的子目錄。|
    |/b|將備份模式中的檔案。|
    |/ copyal|會複製所有檔案資訊，包括資料、 屬性、 時間戳記、 NTFS 存取控制清單 (ACL)、 擁有者資訊及稽核資訊。|
    |/r:6|進行重試此作業六次時就會發生錯誤。|
    |/w:5|等待重試間隔 5 秒。|
    |MT:64|同時複製 64 檔案。|
    |/xd DfsrPrivate|排除 DfsrPrivate 資料夾。|
    |/tee|主控台視窗中，以及在記錄檔中寫入狀態輸出。|
    |/log \ < 記錄檔路徑 >|指定要寫入記錄檔。 覆寫檔案的現有內容。 (要附加至現有的記錄檔的項目，使用`/log+ <log file path>`。)|
    |/v|會產生包含的詳細資訊輸出略過的檔案。|
    
    例如，下列命令將檔案從複製的來源] 資料夾的 [E:\\RF01、 至目的地伺服器上的資料磁碟機 D：
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >我們建議您使用上述當您使用 Robocopy DFS 複寫 preseed 檔案的參數。 不過，您可以變更其值的一些或新增額外的參數。 例如，您可能會找出透過測試有設定較高的值 （執行緒計數） */MT*參數的容量。 此外，如果您將主要複寫較大的檔案，您可能能夠透過新增未緩衝的 I/O **/j**選項增加複製的效能。 如需 Robocopy 參數的詳細資訊，請參閱[Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy)的命令列參考 （英文）。

    >[!WARNING]
    >若要避免可能遺失資料使用 Robocopy DFS 複寫 preseed 檔案時，不要對建議參數進行下列變更：
    >- 請勿使用 */mir*參數 （也就會將樹狀目錄） 或 */mov*參數 （也就會移動檔案，然後刪除來源）。
    >-  請勿移除 **/e**、 **/b**，以及 **/copyall**選項。

4. 複製完成後，查看記錄檔的任何錯誤或跳過的檔案。 使用 Robocopy 複製任何個別而不是複製檔案整組跳過的檔案。 若檔案已略過由於獨佔鎖定嘗試個別檔案複製 Robocopy 與更新版本，或接受這些檔案需要初始同步處理期間的移轉線路複寫 DFS 複寫。

## <a name="next-step"></a>下一步

在完成初始的副本，並盡可能跳過的檔案與解決問題儘可能使用 Robocopy 之後，您要用以**取得 DfsrFileHash**指令程式在 Windows PowerShell 或**Dfsrdiag**命令來比較驗證 preseeded 的檔案來源和目的地伺服器上的檔案雜湊。 如需詳細指示，請參閱[步驟 2： 驗證 Preseeded 檔案 DFS 複寫](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>)。