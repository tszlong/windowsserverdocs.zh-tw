---
title: 使用 Robocopy 將 DFS 複寫預置檔案
description: 如何使用 Robocopy.exe 預置 DFS 複寫的檔案。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: eaec563157a77fd4e782842a81e5b59e49a5ea09
ms.sourcegitcommit: 7cb939320fa2613b7582163a19727d7b77debe4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621295"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>使用 Robocopy 將 DFS 複寫預置檔案

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012 中，Windows Server 2008 R2 及 Windows Server 2008

本主題說明如何使用命令列工具中， **Robocopy.exe**，以在 Windows Server 中設定分散式檔案系統 (DFS) 複寫 （也稱為 DFSR 或 DFS-R） 的複寫時預置檔案。 藉由預先植入的檔案，然後再設定 DFS 複寫，加入新的複寫協力電腦，或取代伺服器，您可以加速初始同步處理，並啟用 Windows Server 2012 R2 中 DFS 複寫資料庫的複製。 Robocopy 方法是其中一種 preseeding 方法;如需概觀，請參閱[步驟 1： 適用於 DFS 複寫預置檔案](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>)。

包含與 Windows Server Robocopy （穩固的檔案複製） 的命令列公用程式。 公用程式會提供廣泛的選項，包括複製安全性、 備份 API 支援，重試功能，以及記錄。 更新版本包含多執行緒處理和未緩衝的 I/O 支援。

>[!IMPORTANT]
>Robocopy 不會複製以獨佔方式鎖定的檔案。 如果使用者通常會在您的檔案伺服器上的長時間鎖定許多檔案，請考慮使用不同的 preseeding 方法。 預先植入，不需要在來源和目的地伺服器上的檔案清單之間完美相符項目，但對於 DFS 複寫，比較沒有效率的預先植入執行初始同步處理時不會存在多個檔案。 發生鎖定衝突降到最低，請在非尖峰時間內，為您的組織使用 Robocopy。 一律檢查 Robocopy 記錄檔之後預先植入，以確保您了解哪些檔案已略過因為獨佔鎖定。

若要使用 Robocopy 預置 DFS 複寫的檔案，請遵循下列步驟：

1. [下載並安裝最新版的 Robocopy。](#step-1-download-and-install-the-latest-version-of-robocopy)
2. [穩固將複寫的檔案。](#step-2-stabilize-files-that-will-be-replicated)
3. [將複寫的檔案複製到目的地伺服器。](#step-3-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>必要條件

因為預先植入，並不直接包含 DFS 複寫，您只需要符合執行使用 Robocopy 檔案複製的需求。

- 您需要在來源和目的地伺服器上本機 Administrators 群組成員的帳戶。

- 您將用來將檔案複製的伺服器上安裝最新版的 Robocopy — 在來源伺服器或目的地伺服器;您必須安裝最新版本的作業系統版本。 如需指示，請參閱[步驟 2:穩定將複寫的檔案](#step-2-stabilize-files-that-will-be-replicated)。 除非您從執行 Windows Server 2003 R2 的伺服器檔案預先植入，您可以在來源或目的地伺服器上執行 Robocopy。 目的地伺服器上，這通常會有較新的作業系統版本，可讓您存取最新版本的 Robocopy。

- 請確定該足夠的儲存空間可在目的地磁碟機上。 不想要複製到路徑上建立資料夾：Robocopy 必須建立根資料夾。
    
    >[!NOTE]
    >當您決定要為 preseeded 檔案配置多少空間時，請考慮預期的資料成長，透過適用於 DFS 複寫的時間和儲存體需求。 如需規劃說明，請參閱[編輯執行資料夾和因衝突而刪除資料夾的配額大小](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11))中[管理 DFS 複寫](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>)。

- 在來源伺服器上，選擇性地安裝處理序監視或處理序總管 中，您可用來檢查正在鎖定檔案的應用程式。 如需下載資訊，請參閱[Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon)並[Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer)。

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>步驟 1：下載並安裝最新版的 Robocopy

使用 Robocopy 預置檔案之前，您應該下載並安裝最新版**Robocopy.exe**。 這可確保 DFS 複寫不會因為 Robocopy 的出貨版本內的問題而略過檔案。

最新的相容 Robocopy 版本的來源取決於伺服器執行的 Windows Server 的版本。 如需下載最新版本的 Robocopy 適用於 Windows Server 2008 R2 或 Windows Server 2008 hotfix 的相關資訊，請參閱[目前可用的 hotfix 清單適用於 Windows Server 2008 中的分散式檔案系統 (DFS) 技術和 Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t)。

或者，您可以找出並執行下列步驟來安裝作業系統的最新的 hotfix。

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>尋找並安裝最新版的 Robocopy 特定版本的 Windows Server

1. 在 web 瀏覽器中，開啟[ https://support.microsoft.com ](https://support.microsoft.com/)。

2. 在 **搜尋支援**，輸入下列字串，取代`<operating system version>`與適當的作業系統，然後按 Enter 鍵：
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    例如，輸入**robocopy.exe kbqfe"Windows Server 2008 R2"** 。

3. 找出並下載具有最高的識別碼號碼 （也就是最新版本） 的 hotfix。

4. 在伺服器上安裝 hotfix。

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>步驟 2：穩定將複寫的檔案

在伺服器上安裝最新版的 Robocopy 之後，您應該使用下表中所述的方法來封鎖複製防止鎖定的檔案。 大部分的應用程式不會以獨佔方式鎖定的檔案。 不過，正常作業期間，一小部分的檔案可能已被鎖定在檔案伺服器上。

|鎖定的來源|說明|緩和措施|
|---|---|---|
|使用者從遠端開啟共用上的檔案。|員工會連接到的標準檔案伺服器，然後編輯文件、 多媒體內容或其他檔案。 有時稱為傳統的主資料夾或共用的資料的工作負載。|僅執行 Robocopy 作業在離峰，在非上班時間。 這樣可以降低 Robocopy 必須預先植入期間略過的檔案數目。<br><br>請考慮暫時設定檔案共用，將會複寫使用 Windows PowerShell 中的 唯讀存取權**授與 SmbShareAccess**並**關閉 SmbSession** cmdlet。 如果您設定一般例如 Everyone 或 Authenticated Users 群組的權限為讀取時，標準使用者可能是較不容易就能開啟檔案的獨佔鎖定 （如果有開啟檔案時，他們的應用程式會偵測的唯讀存取權）。<br><br>您也可以考慮將暫存的防火牆規則設定為 SMB 連接埠 445 輸入至該伺服器，以封鎖檔案的存取權，或使用**區塊 SmbShareAccess** cmdlet。 不過，這兩種方法都非常干擾使用者作業。|
|應用程式會開啟本機檔案。|有時在檔案伺服器上執行的應用程式工作負載會鎖定檔案。|暫時停用或解除安裝正在鎖定檔案的應用程式。 若要判斷哪些應用程式正在鎖定檔案，您可以使用處理序監視或處理序總管。 若要下載處理序監視或處理序總管，請造訪[Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon)並[Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer)頁面。|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>步驟 3：將複寫的檔案複製到目的地伺服器

您可以盡可能減少鎖定的檔案，將會複寫之後，您可以預置從來源伺服器的檔案到目的地伺服器。

>[!NOTE]
>您可以在來源電腦或在目的地電腦上執行 Robocopy。 下列程序說明在目的地伺服器上，這通常執行較新作業系統，才能利用較新的作業系統可能會提供任何其他 Robocopy 功能執行 Robocopy。

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>預置複寫的檔案到目的地伺服器上使用 Robocopy

1. 登入目的地伺服器是在來源和目的地伺服器上本機 Administrators 群組成員的帳戶。

2. 開啟提升權限的命令提示字元。

3. 若要預置檔案從來源到目的地伺服器，執行下列命令，以取代您自己的來源、 目的地和記錄檔案路徑有方括號的值：
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    此命令會將來源資料夾的所有內容都複製到目的地資料夾中，使用下列參數：
    
    |參數|描述|
    |---|---|
    |「\<來源複寫資料夾路徑\>"|指定要在目的地伺服器上預置的來源資料夾。|
    |「\<目的地複寫資料夾路徑\>"|指定將儲存的 preseeded 的檔資料夾的路徑。<br><br>目的資料夾必須在目的地伺服器上已經存在。 若要取得相符的檔案雜湊，當它 preseeds 檔案 Robocopy 時，必須建立根資料夾。|
    |/e|將複製子目錄和其檔案，以及空的子目錄。|
    |/b|將檔案複製備份 」 模式。|
    |/copyall|複製所有的檔案資訊，包括資料、 屬性、 時間戳記、 NTFS 存取控制清單 (ACL)、 擁有者資訊和稽核資訊。|
    |/r:6|重試作業六次發生錯誤時。|
    |/w:5|會等候 5 秒，重試之間。|
    |MT:64|同時複製 64 的檔案。|
    |/xd DfsrPrivate|排除 DfsrPrivate 資料夾。|
    |/tee|主控台視窗中，以及記錄檔，請將寫入狀態輸出。|
    |/ 記錄\<記錄檔路徑 >|指定要寫入記錄檔。 會覆寫檔案的現有內容。 (若要將項目附加至現有的記錄檔中，使用`/log+ <log file path>`。)|
    |/v|產生的詳細資訊輸出，其中包含略過的檔案。|
    
    比方說，下列命令會將檔案複寫在複寫的來源資料夾中，e:\\RF01，到目的地伺服器上的資料磁碟機 D:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >我們建議您使用上面所述，當 DFS 複寫預置檔案的情況下，您在使用 Robocopy 的參數。 不過，您可以變更其值的部分，或新增其他參數。 例如，您可能會找出透過測試您已設定較高的值 （執行緒計數） 的容量 */MT*參數。 此外，如果您將主要複寫較大的檔案，您可能可以提升複製效能，加上 **/j**無緩衝 I/O 的選項。 如需有關 Robocopy 參數的詳細資訊，請參閱[Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy)命令列的參考。

    >[!WARNING]
    >若要避免資料遺失，當 DFS 複寫預置檔案的情況下，您在使用 Robocopy，不要對 建議的參數進行下列變更：
    >- 請勿使用 */mir*參數 （可反映樹狀目錄） 或 */mov*參數 （將會移動檔案，然後從來源中刪除）。
    >-  請勿移除 **/e**， **b**，並 **/copyall**選項。

4. 複製完成之後，請檢查記錄檔的任何錯誤或略過的檔案。 您可以使用 Robocopy 來複製任何略過的檔案，個別而不是重新複製完整的檔案。 如果因為獨佔鎖定略過的檔案，再次嘗試複製個別檔案 Robocopy 與更新版本中，或接受這些檔案必須在初始同步處理期間由 「 DFS 複寫的移轉網路複寫。

## <a name="next-step"></a>後續步驟

在完成初始複本，並使用 Robocopy 將解決問題的多個略過的檔案，以盡可能之後，您將使用**Get DfsrFileHash**在 Windows PowerShell cmdlet 或**Dfsrdiag**命令藉由比較來源和目的地伺服器上的檔案雜湊驗證 preseeded 的檔案。 如需詳細指示，請參閱[步驟 2:適用於 DFS 複寫驗證 Preseeded 的檔案](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>)。
