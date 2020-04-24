---
title: 使用 Robocopy 預先植入用於 DFS 複寫的檔案
description: 如何使用 Robocopy.exe 預先植入用於 DFS 複寫的檔案。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 8ff800fc2a0885cec39ca104607d7207f0bd8ce0
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80815601"
---
# <a name="use-robocopy-to-pre-seed-files-for-dfs-replication"></a>使用 Robocopy 預先植入用於 DFS 複寫的檔案

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

本主題說明如何使用命令列工具 (**Robocopy.exe**)，在 Windows Server 中設定適用於分散式檔案系統 (DFS) 複寫 (也稱為 DFSR 或 DFS) 的複寫時預先植入檔案。 藉由在設定 DFS 複寫、新增新複寫夥伴或取代伺服器之前先預先植入檔案，可加速初始同步，並在 Windows Server 2012 R2 中啟用 DFS 複寫資料庫的複製作業。 Robocopy 方法是數個預先植入方法的其中一種；如需了解概觀，請參閱[步驟1：預先植入用於 DFS 複寫的檔案](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>)。

Windows Server 中隨附 Robocopy (Robust File Copy) 命令列公用程式。 此公用程式提供許多選項，包括複製安全性、備份 API 支援、重試功能和記錄。 較新的版本則納入了多重執行緒和未緩衝處理的 I/O 支援。

>[!IMPORTANT]
>Robocopy 不會複製獨佔鎖定的檔案。 如果使用者想在檔案伺服器上長時間鎖定許多檔案，請考慮使用其他的預先植入方法。 預先植入不需要來源與目的地伺服器上的檔案清單能完全相符，但執行 DFS 複寫的初始同步時，不存在的檔案越多，預先植入的效率就愈低。 若要將鎖定衝突降到最低，請在貴組織的非尖峰時間使用 Robocopy。 請務必在預先植入之後檢查 Robocopy 記錄，確保您知道有哪些檔案因為獨佔鎖定而略過。

若要使用 Robocopy 預先植入用於 DFS 複寫的檔案，請遵循下列步驟：

1. [下載和安裝 OneGet 安裝最新版本的 Robocopy。](#step-1-download-and-install-the-latest-version-of-robocopy)
2. [將要複寫的檔案穩定化。](#step-2-stabilize-files-that-will-be-replicated)
3. [將複寫的檔案複製到目的地伺服器。](#step-3-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>必要條件

由於預先植入不會直接牽涉到 DFS 複寫，因此您只需符合使用 Robocopy 執行檔案複製的需求。

- 您需要同時是來源和目的地伺服器上本機系統管理員群組成員的帳戶。

- 將用來複製檔案的伺服器 (來源伺服器或目的地伺服器) 上安裝最新版本的 Robocopy 時，您需要安裝適用於該作業系統版本的最新版本。 如需指示，請參閱[步驟 2：將要複寫的檔案穩定化](#step-2-stabilize-files-that-will-be-replicated)。 除非您要從執行 Windows Server 2003 R2 的伺服器預先植入檔案，否則可以選擇在來源或目的地伺服器上執行 Robocopy。 目的地伺服器 (通常具有較新的作業系統版本) 可讓您存取最新版本的 Robocopy。

- 確定目的地磁碟有足夠的儲存空間。 請勿在您打算複製的路徑上建立資料夾：Robocopy 必須建立根資料夾。
    
    >[!NOTE]
    >決定要為預先植入檔案配置多少空間時，請將預期會隨時間成長的資料量和 DFS 複寫的儲存需求列入考量。 若需要協助以進行規劃，請參閱[編輯 DFS 複寫](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>)中的[編輯複寫用快取資料夾和因衝突而刪除資料夾的配額大小](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11))。

- 在來源伺服器上，選擇性地安裝處理序監視器或處理序總管，您可以使用此工具來檢查鎖定檔案的應用程式。 如需下載資訊，請參閱[處理序監視器](https://docs.microsoft.com/sysinternals/downloads/procmon)和[處理序總管](https://docs.microsoft.com/sysinternals/downloads/process-explorer)。

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>步驟 1：下載和安裝 OneGet 安裝最新版本的 Robocopy

使用 Robocopy 預先植入檔案前，請先下載並安裝最新版的 **Robocopy.exe**。 這可確保 DFS 複寫不會因為 Robocopy 發行版本內的問題而略過檔案。

最新相容 Robocopy 版本來源取決於伺服器上執行的 Windows Server 版本。 如需下載包含 Windows Server 2008 R2 或 Windows server 2008 最新版本 Robocopy 的 Hotfix 相關資訊，請參閱 [Windows server 2008 和 Windows Server 2008 R2 中目前可用的分散式檔案系統 (DFS) 技術清單](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t)。

或者，您可以採取下列步驟，找出並安裝適用於作業系統的最新 Hotfix。

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>找出並安裝適用於特定版本之 Windows Server 的最新版本 Robocopy

1. 在網頁瀏覽器中，開啟 [https://support.microsoft.com](https://support.microsoft.com/)。

2. 在 [搜尋支援]  中輸入下列字串，以適當的作業系統取代 `<operating system version>`，然後按下 Enter 鍵：
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    例如，輸入 **robocopy.exe kbqfe "Windows Server 2008 R2"** 。

3. 找出並下載具有最大識別碼 (也就是最新版本) 的 Hotfix。

4. 在伺服器上安裝 Hotfix。

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>步驟 2：將要複寫的檔案穩定化

在伺服器上安裝最新版本的 Robocopy 後，請使用下表所述的方法，防止鎖定的檔案封鎖複製作業。 大部分的應用程式都不會獨佔鎖定檔案。 不過，在正常作業期間，檔案伺服器上可能會有少數檔案遭到鎖定。

|鎖定的來源|說明|緩和措施|
|---|---|---|
|使用者從遠端開啟共用上的檔案。|員工連線至標準檔案伺服器並編輯文件、多媒體內容或其他檔案。 有時系統會將其當做傳統主資料夾或共用資料工作負載參考。|只在離峰期間 (非工作時間) 執行 Robocopy 作業。 這可將 Robocopy 在預先植入期間必須略過的檔案數目降至最低。<br><br>請考慮暫時設定檔案共用上的唯讀存取權，這些檔案共用會使用 Windows PowerShell **參考** 和 **Close-SmbSession** Cmdlet 進行複寫。 如果將一般群組 (例如每個人或已驗證的使用者) 的權限設為「讀取」，標準使用者較不可能開啟獨佔鎖定的檔案 (如果其應用程式在檔案開啟時偵測到唯讀存取)。<br><br>您也可以考慮將 SMB 連接埠 445 傳入的暫時防火牆規則設定為該伺服器，以封鎖對檔案的存取，或使用 **Block-SmbShareAccess** Cmdlet。 不過，這兩種方法都會干擾使用者作業。|
|應用程式在本機開啟檔案。|在檔案伺服器上執行的應用程式工作負載有時會鎖定檔案。|暫時停用或將鎖定檔案的應用程式解除安裝。 您可以使用處理序監視器或處理序總管來判斷鎖定檔案的應用程式。 若要下載處理序監視或處理序總管，請造訪[處理序監視](https://docs.microsoft.com/sysinternals/downloads/procmon)和[處理序總管](https://docs.microsoft.com/sysinternals/downloads/process-explorer)頁面。|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>步驟 3：將複寫的檔案複製到目的地伺服器

將要複寫的檔案鎖定降到最低之後，您可以將檔案從來源伺服器預先植入目的地伺服器。

>[!NOTE]
>您可以在來源電腦或目的電腦上執行 Robocopy。 下列程序說明如何在目的地伺服器上執行 Robocopy，這通常會執行較新的作業系統，以利用最新作業系統可能提供的任何其他 Robocopy 功能。

### <a name="pre-seed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>使用 Robocopy 將複寫的檔案預先植入目的地伺服器

1. 使用同時是來源和目的地伺服器上本機系統管理員群組成員的帳戶，登入目的地伺服器。

2. 開啟提升權限的命令提示字元。

3. 若要從來源將檔案預先植入目的地伺服器，請執行下列命令，並以您自己的來源、目的地和記錄檔路徑取代括弧中的值：
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    此命令會使用下列參數，將來源資料夾的所有內容複製到目的地資料夾：
    
    |參數|說明|
    |---|---|
    |"\<source replicated folder path\>"|指定要在目的地伺服器上預先植入的原始檔案。|
    |"\<destination replicated folder path\>"|指定將儲存預先植入檔案的資料夾路徑。<br><br>目的地伺服器中不能存在目的地資料夾。 若要取得相符的檔案雜湊，Robocopy 必須在預先植入檔案時建立根資料夾。|
    |/e|複製子目錄和其檔案，以及空白的子目錄。|
    |/b|以備份模式複製檔案。|
    |/copyall|複製所有檔案資訊，包括資料、屬性、時間戳記、NTFS 存取控制清單 (ACL)、擁有者資訊和稽核資訊。|
    |/r:6|發生錯誤時，重試作業六次。|
    |/w:5|在重試之間等待 5 秒。|
    |MT:64|同時複製 64 個檔案。|
    |/xd DfsrPrivate|排除 DfsrPrivate 資料夾。|
    |/tee|將狀態輸出寫入主控台視窗及記錄檔。|
    |/log \<log file path>|指定要寫入的記錄檔。 覆寫檔案的現有內容。 (若要將項目附加至現有的記錄檔，請使用 `/log+ <log file path>`)。|
    |/v|產生詳細資訊輸出，其中包含略過的檔案。|
    
    例如，下列命令會將來源複寫資料夾 (E:\\RF01) 中的檔案複寫到目的地伺服器上的資料磁碟機 D：
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\pre-seedsrv02.log
    ```
    
    >[!NOTE]
    >使用 Robocopy 來預先植入檔案以執行 DFS 複寫時，建議您使用上述的參數。 不過，您可以變更其中一些參數值，或新增其他參數。 例如，經測試後您可能會發現可為 */MT* 參數設定較高的值 (執行緒計數)。 此外，如果您主要會複寫較大的檔案，可以為未緩衝的 I/O 新增 **/j** 選項，以提高複製效能。 如需有關 Robocopy 參數的詳細資訊，請參閱 [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) 命令列參考。

    >[!WARNING]
    >若要避免在使用 Robocopy 將檔案預先植入以執行 DFS 複寫時遺失資料，請不要對建議的參數進行下列變更：
    >- 請勿使用 */mir* 參數 (會將目錄樹狀結構鏡像處理) 或 */mov* 參數 (會移動檔案，然後從來源刪除這些檔案)。
    >-  請勿移除 **/e**、 **/b** 和 **/copyall** 選項。

4. 複製完成之後，請檢查記錄中是否有任何錯誤或略過的檔案。 使用 Robocopy 分別複製任何略過的檔案，而不要重新複製整組檔案。 如果系統因為獨佔鎖定而略過檔案，請稍後嘗試使用 Robocopy 個別複製檔案，或在初始同步期間，接受檔案需要透過網路使用 DFS 複寫執行複寫。

## <a name="next-step"></a>後續步驟

完成初始複製，並使用 Robocopy 解決盡可能略過的檔案數問題時，會使用 Windows PowerShell 中的 **Get-DfsrFileHash** Cmdlet 或 **Dfsrdiag** 命令，以比較來源與目的地伺服器上的檔案雜湊來驗證預先植入的檔案。 如需詳細指示，請參閱[步驟 2：驗證用於 DFS 複寫的預先植入檔案](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>)。
