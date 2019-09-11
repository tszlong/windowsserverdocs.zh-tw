---
title: 使用 Robocopy 來預置 DFS 複寫的檔案
description: 如何使用 Robocopy 預置檔案以進行 DFS 複寫。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4a0cad3c685c8609784c7096fe31d55294712c2e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871984"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>使用 Robocopy 來預置 DFS 複寫的檔案

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

本主題說明如何使用命令列工具**Robocopy .exe**，在 Windows Server 中設定分散式檔案系統（DFS）複寫（也稱為 DFSR 或預置）的複寫時，用來檔案。 藉由在設定 DFS 複寫、新增複寫協力電腦或取代伺服器之前先 preseeding 檔案，您可以加速初始同步處理，並在 Windows Server 2012 R2 中啟用複製 DFS 複寫資料庫。 Robocopy 方法是數個 preseeding 方法的其中一種;如需總覽，請參閱[步驟1：預置 files for DFS 複寫](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>)。

Robocopy （健全的檔案複製）命令列公用程式隨附于 Windows Server。 公用程式提供廣泛的選項，包括複製安全性、備份 API 支援、重試功能和記錄。 較新的版本包括多重執行緒和未緩衝處理的 i/o 支援。

>[!IMPORTANT]
>Robocopy 不會複製獨佔鎖定的檔案。 如果使用者傾向于在檔案伺服器上長時間鎖定許多檔案，請考慮使用不同的 preseeding 方法。 Preseeding 不需要在來源與目的地伺服器上的檔案清單之間進行完美的比對，但是針對 DFS 複寫執行初始同步處理時，不存在的檔案越多，Preseeding 的效率就愈低。 若要將鎖定衝突降到最低，請在貴組織的非尖峰時間使用 Robocopy。 請一律檢查 preseeding 後的 Robocopy 記錄，以確保您瞭解因為獨佔鎖定而略過的檔案。

若要使用 Robocopy 來預置 DFS 複寫的檔案，請遵循下列步驟：

1. [下載並安裝最新版本的 Robocopy。](#step-1-download-and-install-the-latest-version-of-robocopy)
2. [將複寫的穩定檔案。](#step-2-stabilize-files-that-will-be-replicated)
3. [將複寫的檔案複製到目的地伺服器。](#step-3-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>必要條件

由於 preseeding 不會直接牽涉到 DFS 複寫，因此您只需要符合使用 Robocopy 執行檔案複製的需求。

- 您需要的帳戶是來源和目的地伺服器上本機系統管理員群組的成員。

- 在您將用來複製檔案的伺服器（來源伺服器或目的地伺服器）上，安裝 Robocopy 的最新版本。您將需要安裝作業系統版本的最新版本。 如需指示， [請參閱步驟2：將複寫的穩定檔案](#step-2-stabilize-files-that-will-be-replicated)。 除非您要從執行 Windows Server 2003 R2 的伺服器 preseeding 檔案，否則您可以在來源或目的地伺服器上執行 Robocopy。 目的地伺服器（通常具有較新的作業系統版本）可讓您存取最新版本的 Robocopy。

- 確定目的地磁片磁碟機上有足夠的儲存空間。 請勿在您打算複製的路徑上建立資料夾：Robocopy 必須建立根資料夾。
    
    >[!NOTE]
    >當您決定要為 preseeded 檔案配置多少空間時，請考慮隨時間變化的預期資料成長和 DFS 複寫的儲存需求。 如需規劃說明，請參閱在[管理 DFS 複寫](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>)中[編輯執行資料夾的配額大小和衝突與已刪除的資料夾](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11))。

- 在來源伺服器上，選擇性地安裝進程監視器或進程瀏覽器，您可以使用此工具來檢查鎖定檔案的應用程式。 如需下載資訊，請參閱[進程監視](https://docs.microsoft.com/sysinternals/downloads/procmon)和[進程瀏覽器](https://docs.microsoft.com/sysinternals/downloads/process-explorer)。

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>步驟 1：下載並安裝最新版本的 Robocopy

使用 Robocopy 預置檔案之前，您應該下載並安裝最新版本的**Robocopy .exe**。 這可確保 DFS 複寫不會因為 Robocopy 運送版本內的問題而略過檔案。

最新相容 Robocopy 版本的來源取決於伺服器上執行的 Windows Server 版本。 如需針對 Windows Server 2008 R2 或 Windows Server 2008 下載含有最新 Robocopy 版本的修正程式的詳細資訊，請參閱[Windows server 2008 和中的分散式檔案系統（DFS）技術的目前可用的修補程式清單Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t)。

或者，您可以採取下列步驟，找出並安裝作業系統的最新的修補程式。

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>針對特定版本的 Windows Server 找出並安裝最新版本的 Robocopy

1. 在網頁瀏覽器中， [https://support.microsoft.com](https://support.microsoft.com/)開啟。

2. 在 [**搜尋支援**] 中，輸入下列字串`<operating system version>` ，將取代為適當的作業系統，然後按 enter 鍵：
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    例如，輸入**robocopy kbqfe "Windows Server 2008 R2"** 。

3. 找出並下載具有最高 ID 號碼的修補程式（也就是最新版本）。

4. 在伺服器上安裝此修補程式。

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>步驟 2：將複寫的穩定檔案

在伺服器上安裝最新版本的 Robocopy 之後，您應該使用下表所述的方法，防止鎖定的檔案封鎖複製。 大部分的應用程式都不會獨佔鎖定檔案。 不過，在正常作業期間，檔案伺服器上可能會鎖定少量的檔案。

|鎖定的來源|說明|緩和措施|
|---|---|---|
|使用者從遠端開啟共用上的檔案。|員工連線至標準檔案伺服器，並編輯檔、多媒體內容或其他檔案。 有時稱為傳統主資料夾或共用資料工作負載。|只在離峰期間（非上班時間）執行 Robocopy 作業。 這可將 Robocopy 在 preseeding 期間必須略過的檔案數目降至最低。<br><br>請考慮使用 Windows PowerShell **Grant-SmbShareAccess**和**SmbSession** Cmdlet，暫時設定要複寫之檔案共用的唯讀存取權。 如果您設定一般群組（例如每個人或已驗證的使用者）的許可權，標準使用者可能較不可能開啟具有獨佔鎖定的檔案（如果其應用程式在檔案開啟時偵測到唯讀存取）。<br><br>您也可以考慮將 SMB 埠445輸入的暫時防火牆規則設定為該伺服器，以封鎖對檔案的存取，或使用**SmbShareAccess**指令程式。 不過，這兩種方法對使用者作業非常有干擾。|
|應用程式會開啟本機檔案。|在檔案伺服器上執行的應用程式工作負載有時會鎖定檔案。|暫時停用或卸載正在鎖定檔案的應用程式。 您可以使用進程監視器或進程瀏覽器來判斷哪些應用程式正在鎖定檔案。 若要下載流程監視或進程瀏覽器，請造訪[進程監視器](https://docs.microsoft.com/sysinternals/downloads/procmon)和[進程瀏覽器](https://docs.microsoft.com/sysinternals/downloads/process-explorer)頁面。|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>步驟 3：將複寫的檔案複製到目的地伺服器

將複寫的檔案的鎖定降到最低之後，您可以將檔案從來源伺服器預置到目的地伺服器。

>[!NOTE]
>您可以在來源電腦或目的地電腦上執行 Robocopy。 下列程式說明如何在目的地伺服器上執行 Robocopy，這通常會執行較新的作業系統，以利用最新作業系統可能提供的任何其他 Robocopy 功能。

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>使用 Robocopy 將複寫的檔案預置到目的地伺服器

1. 使用同時為來源和目的地伺服器上本機系統管理員群組成員的帳戶登入目的地伺服器。

2. 開啟提升權限的命令提示字元。

3. 若要從來源預置檔案到目的地伺服器，請執行下列命令，並將您自己的來源、目的地和記錄檔路徑取代為加上括弧的值：
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    此命令會使用下列參數，將源資料夾的所有內容複寫到目的地資料夾：
    
    |參數|描述|
    |---|---|
    |「來源複寫資料夾路徑\>」 \<|指定要在目的地伺服器上預置的原始檔案。|
    |「目的地複寫資料夾路徑\>」 \<|指定將儲存 preseeded 檔案的資料夾路徑。<br><br>目的地伺服器上不能已有目的地資料夾。 若要取得相符的檔案雜湊，Robocopy 必須在 preseeds 檔案時建立根資料夾。|
    |/e|複製子目錄和其檔案，以及空白的子目錄。|
    |/b|以備份模式複製檔案。|
    |/copyall|複製所有檔案資訊，包括資料、屬性、時間戳記、NTFS 存取控制清單（ACL）、擁有者資訊和審核資訊。|
    |/r：6|發生錯誤時，重試作業六次。|
    |/w：5|在重試之間等待5秒。|
    |MT：64|同時複製64檔案。|
    |/xd DfsrPrivate|排除 DfsrPrivate 資料夾。|
    |/tee|將狀態輸出寫入主控台視窗，以及記錄檔。|
    |/log \<記錄檔路徑 >|指定要寫入的記錄檔。 覆寫檔案的現有內容。 （若要將專案附加至現有的記錄檔， `/log+ <log file path>`請使用）。|
    |/v|產生詳細資訊輸出，其中包含略過的檔案。|
    
    例如，下列命令會將來源複寫資料夾（E：\\RF01）中的檔案複寫到目的地伺服器上的資料磁片磁碟機 D：
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >當您使用 Robocopy 來預置檔案以進行 DFS 複寫時，建議您使用上述的參數。 不過，您可以變更其中一些值，或新增其他參數。 例如，您可能會發現測試，您有容量可為 */mt*參數設定較高的值（執行緒計數）。 此外，如果您主要會複寫較大的檔案，您可以新增未緩衝 i/o 的 **/j**選項來增加複製效能。 如需 Robocopy 參數的詳細資訊，請參閱[robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy)命令列參考。

    >[!WARNING]
    >若要避免在使用 Robocopy 預置檔案以進行 DFS 複寫時可能遺失資料，請不要對建議的參數進行下列變更：
    >- 請勿使用 */mir*參數（其會鏡像目錄樹狀結構）或 */mov*參數（這會移動檔案，然後從來源刪除這些檔案）。
    >-  請勿移除 [ **/e**]、[ **/b**] 和 [ **/copyall** ] 選項。

4. 複製完成之後，請檢查記錄檔中是否有任何錯誤或略過的檔案。 使用 Robocopy 來個別複製任何略過的檔案，而不是正在整組檔。 如果因為獨佔鎖定而略過檔案，請稍後嘗試使用 Robocopy 複製個別檔案，或在初始同步處理期間，接受那些檔案需要透過網路複寫 DFS 複寫。

## <a name="next-step"></a>後續步驟

當您完成初始複製，並使用 Robocopy 來解決多個略過檔案的問題時，您會在 Windows PowerShell 或**Dfsrdiag**命令中使用**DfsrFileHash** Cmdlet，藉由比較來驗證 preseeded 檔案來源和目的地伺服器上的檔案雜湊。 如需詳細指示， [請參閱步驟2：驗證 DFS 複寫](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>)的 Preseeded 檔。
