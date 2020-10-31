---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: 虛擬網域控制站的疑難排解
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 28e1f82322389e7b46b7d597b6657f9512ab011f
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071260"
---
# <a name="virtualized-domain-controller-troubleshooting"></a>虛擬網域控制站的疑難排解

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

這個主題提供疑難排解虛擬網域控制站功能的詳細方法。

- [疑難排解虛擬網域控制站的複製功能](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)

- [疑難排解虛擬網域控制站的安全還原功能](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)

## <a name="introduction"></a><a name="BKMK_Intro"></a>簡介
改善疑難排解技術的最重要方式是建置測試實驗室，並嚴格檢查正常運作的案例。 由於您已經具備如何升級網域控制站的穩固基礎，因此，當您遇到錯誤時，就能夠更明顯地看出並更容易了解錯誤。 這也有助於您建立分析與網路分析技術。 這適用於所有分散式系統技術，而不只是虛擬網域控制站的部署。

進階疑難排解網域控制站設定的重要元素如下：

1. 結合重點的線性分析，並注意詳細資料。

2. 了解網路擷取分析

3. 了解內建的記錄檔

前兩項已超出這個主題的範圍，但是這個主題會詳盡說明第三項。 疑難排解虛擬網域控制站需要有一個邏輯線性方法。 重點在於使用提供的資料來處理問題，並且只有在您用盡提供的輸出與記錄時，才採用複雜的工具與分析。

## <a name="troubleshooting-virtualized-domain-controller-cloning"></a><a name="BKMK_TshootVDCCloning"></a>疑難排解虛擬網域控制站的複製功能
本節包含：

- [疑難排解的工具](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)

- [記錄選項](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)

- [適用於疑難排解虛擬網域控制站複製功能的一般方法](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)

- [Server Core 和事件記錄檔](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)

- [疑難排解特定問題](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)

適合用來疑難排解虛擬網域控制站複製功能的策略會遵循下列通用格式：

![虛擬 dc 疑難排解](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)

### <a name="tools-for-troubleshooting"></a><a name="BKMK_Tools"></a>疑難排解的工具

#### <a name="logging-options"></a><a name="BKMK_LoggingOptions"></a>記錄選項

內建的記錄檔是用來疑難排解網域控制站複製問題的最重要工具。 預設會啟用並設定這所有的記錄檔，以取得最多的詳細資料。

| **運算** | **Log** |
|--|--|
| **複製** | -事件檢視器 \windows 記錄 \ 系統<br />-事件 viewer\Applications 和服務 logs\Directory 服務<br />- %systemroot%\debug\dcpromo.log |
| **促銷** | - %systemroot%\debug\dcpromo.log<br />-事件 viewer\Applications 和服務 logs\Directory 服務<br />-事件檢視器 \windows 記錄 \ 系統<br />-事件 viewer\Applications 和服務 logs\File Replication Service<br />-事件 viewer\Applications 和服務 logs\DFS 複寫 |

#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>適用於疑難排解網域控制站設定的工具和命令
若要疑難排解記錄檔未說明的問題，請使用下列工具做為起點：

- Dcdiag.exe

- Repadmin.exe

- 網路監視器 3.4

### <a name="general-methodology-for-troubleshooting-domain-controller-cloning"></a><a name="BKMK_GeneralMethodology"></a>適用於疑難排解虛擬網域控制站複製功能的一般方法

1. VM 是否正開機進入 DS 修復模式 (DSRM)？ 這表示必須進行疑難排解。 若要在 DSRM 中登入，請使用 **.\Administrator** 帳戶並指定 DSRM 密碼。

    1. 檢查 Dcpromo.log。

        1. 初始複製步驟成功，但是網域控制站升級失敗？

        2. 是否有錯誤指出是本機網域控制站的問題，或是 AD DS 環境的問題，例如，從 PDC 模擬器傳回錯誤？

    2. 檢查 [系統] 和 [目錄服務] 事件記錄檔，以及 dccloneconfig.xml 和 CustomDCCloneAllowList.xml

        1. 不相容的應用程式是否需要位於 CustomDCCloneAllowList.xml 允許清單中？

        2. IP 位址或電腦名稱在 dccloneconfig.xml 中是否重複或無效？

        3. Active Directory 站台在 dccloneconfig.xml 中是否無效？

        4. 未在 dccloningconfig.xml 中設定 IP 位址，而且沒有 DHCP 伺服器可供使用？

        5. PDC 模擬器是否已上線且可透過 RPC 通訊協定加以使用？

        6. 網域控制站是 [可複製的網域控制站] 群組的成員嗎？ 已在該群組的網域根目錄上設定 [允許 DC 建立本身的複製品]  的權限嗎？

        7. Dccloneconfig.xml 檔案是否包含了妨礙正確剖析進行的語法錯誤？

        8. 是否支援 Hypervisor？

        9. 網域控制站升級在複製成功開始之後失敗？

        10. 已超過自動產生的網域控制站名稱的數目上限 (9999) 嗎？

        11. MAC 位址重複？

2. 複製品的主機名稱與來源 DC 相同嗎？

    1. 是否有某一個允許的位置中具有 Dccloneconfig.xml 檔案？

3. VM 正開機進入標準模式且已完成複製，但是網域控制站無法正確運作嗎？

    1. 首先檢查複製品中的主機名稱是否已變更。 如果主機名稱不同，至少已完成部分複製。

    2. 網域控制站是否具備來自 dccloneconfig.xml 的來源網域控制站的重複 IP 位址，但是來源網域控制站在複製期間已離線？

    3. 如果網域控制站正在進行通告，請將這個問題視為您在未進行複製時所遇到的任何正常升級後問題。

    4. 如果網域控制站並未進行通告，請檢查 [目錄服務]、[系統]、[應用程式]、[檔案複寫]，以及 [DFS 複寫] 事件記錄檔，以取得升級後的錯誤。

#### <a name="disabling-dsrm-boot"></a>停用 DSRM 開機
一旦因為發生任何錯誤而開機進入 DSRM 之後，請診斷發生失敗的原因，而且如果 dcpromo.log 未指出無法重試複製，請修正發生失敗的原因並重設 DSRM 旗標。 失敗的複製品不會在下一次重新開機時自行返回正常模式；您必須移除 DS 還原模式開機旗標，然後再次嘗試複製。 這所有的步驟都需要以提升權限的系統管理員身分執行。

##### <a name="removing-dsrm-with-msconfigexe"></a>使用 Msconfig.exe 移除 DSRM
若要使用 GUI 關閉 DSRM 開機功能，請使用 [系統設定] 工具：

1. 執行 msconfig.exe

2. 在 [開機]  索引標籤的 [開機選項]  下方，取消選取 [安全開機]  (如果已啟用 [Active Directory 修復]  選項，則會選取此選項)。

3. 按一下 [確定]，在出現提示時重新啟動

##### <a name="removing-dsrm-with-bcdeditexe"></a>使用 Bcdedit.exe 移除 DSRM
若要從命令列關閉 DSRM 開機功能，請使用 [開機設定資料存放區編輯器]：

1. 開啟 CMD 命令提示字元並執行：

    ```
    Bcdedit.exe /deletevalue safeboot
    ```

2. 使用下列命令重新啟動電腦：

    ```
    Shutdown.exe /t /0 /r
    ```

> [!NOTE]
> Bcdedit.exe 也可以在 Windows PowerShell 主控台中運作。 命令如下：
>
> Bcdedit.exe /deletevalue safeboot
>
> Restart-computer

### <a name="server-core-and-the-event-log"></a><a name="BKMK_ServerCoreEvents"></a>Server Core 和事件記錄檔
事件記錄檔包含許多關於虛擬網域控制站複製作業的有用資訊。 根據預設，Windows Server 2012 電腦安裝是 Server Core 安裝，這表示沒有圖形化介面，因此，不能執行本機 [事件檢視器] 嵌入式管理單元。

在執行 Server Core 安裝的伺服器上檢閱事件記錄檔：

- 在本機執行 Wevtutil.exe 工具

- 在本機執行 PowerShell Cmdlet Get-WinEvent

- 如果您已啟用「遠端事件日誌管理」群組 (或對等埠) 的 Windows Advanced 防火牆規則，則可以使用 Eventvwr.exe、wevtutil.exe 或 New-winevent，從遠端管理事件記錄檔。 這可以在 Server Core 安裝中，使用 NETSH.exe、群組原則或 Windows PowerShell 3.0 中的新 Set-NetFirewallRule Cmdlet 來完成。

> [!WARNING]
> 請勿在電腦處於 DSRM 時，嘗試將圖形化介面新增回電腦。 Windows 服務堆疊 (CBS) 無法在安全模式或 DSRM 中正常運作。 嘗試在處於 DSRM 時新增功能或角色將無法完成，而且會讓電腦在重新開機之前都處於不穩定狀態。 由於 DSRM 中的虛擬網域控制站複製品無法正常開機，而且在大部分情況下應該都無法正常開機，因此無法安全新增圖形化介面。 此動作不受支援，而且可能使得伺服器無法使用。

### <a name="troubleshooting-specific-problems"></a><a name="BKMK_SpecificProblems"></a>疑難排解特定問題

#### <a name="events"></a>事件
所有的虛擬網域控制站複製事件都會寫入複製網域控制站 VM 的 [目錄服務] 事件記錄檔。 [應用程式]、[檔案複寫服務]，以及 [DFS 複寫] 事件記錄檔可能也會包含關於複製失敗的有用疑難排解資訊。 在 PDC 模擬器的 RPC 呼叫期間的失敗或許可以在 PDC 模擬器的事件記錄檔中取得。

以下是 [目錄服務] 事件記錄檔中 Windows Server 2012 複製特定的事件，其中含有錯誤的附註和建議解決方案。

##### <a name="directory-services-event-log"></a>目錄服務事件記錄檔

| 事件 | 描述 |
| -- |--|
| **事件識別碼** | **2160** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message**
| 本機 *<COMPUTERNAME>* 已找到虛擬網域控制站複製設定檔。<p>此虛擬網域控制站複製設定檔位於：%1<p>虛擬網域控制站複製設定檔的存在，表示本機虛擬網域控制站是另一個虛擬網域控制站的複製品。 *<COMPUTERNAME>* 將開始自行複製。 |
 **注意事項與解決方法** | 這是成功的事件，只有在發生未預期的狀況時才會成為問題。 檢查 DSA 工作目錄 %systemroot%\ntds，以及任何本機或卸除式磁碟的根目錄，以取得 dcclconeconfig.xml 檔案。 |

| 事件 | 描述 |
| -- |--|
| **事件識別碼** | **2161** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | 本機找 *<COMPUTERNAME>* 不到虛擬網域控制站複製設定檔。 本機電腦不是複製的 DC。| **注意事項與解決方法** | 這是成功的事件，只有在發生未預期的狀況時才會成為問題。 檢查 DSA 工作目錄 %systemroot%\ntds，以及任何本機或卸除式磁碟的根目錄，以取得 dcclconeconfig.xml 檔案。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2162** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | 虛擬網域控制站複製失敗。<p>請檢查 [系統] 事件記錄檔與 %systemroot%\debug\dcpromo.log 中記錄的事件，了解有關虛擬網域控制站複製嘗試錯誤的詳細資訊。<p>錯誤碼：%1 |
| **注意事項與解決方法** | 依照訊息指示執行，這個錯誤是一個 catchall。 |

| 事件 | 描述 |
| -- |--|
|**事件識別碼**|**2163**|
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|
|**嚴重性**|資訊|
|**Message**|已啟動 DsRoleSvc 服務以複製本機虛擬網域控制站。|
|**注意事項與解決方法**|這是成功的事件，只有在發生未預期的狀況時才會成為問題。 檢查 DSA 工作目錄 %systemroot%\ntds，以及任何本機或卸除式磁碟的根目錄，以取得 dcclconeconfig.xml 檔案。|

| 事件 | 描述 |
| -- |--|
| **事件識別碼** | **2164** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 無法啟動 DsRoleSvc 服務，以複製本機虛擬網域控制站。 |
| **注意事項與解決方法** | 檢查「DS 角色伺服器」服務 (DsRoleSvc) 的服務設定，並確定已將它的啟動類型設為 [手動]。 請確定沒有任何協力廠商程式會防止這個服務啟動。 |

| 事件 | 描述 |
| -- |--|
| **事件識別碼** | **2165** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 在複製本機虛擬網域控制站期間，無法啟動執行緒。<p>錯誤碼：%1<p>錯誤訊息：%2<p>執行緒名稱：%3 |
| **注意事項與解決方法** | 請連絡 Microsoft 產品支援 |

| 事件 | 描述 |
| -- |--|
| **事件識別碼** | **2166** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 需要 RPCSS 服務，才能起始重新開機到 DSRM。 等待 RPCSS 初始化為執行狀態失敗。<p>錯誤碼：%1 |
| **注意事項與解決方法** | 檢查 [系統] 事件記錄檔和 [RPC 伺服器] 服務 (Rpcss) 的服務設定 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2168** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | Microsoft-Windows-ActiveDirectory_DomainService<p>DC 正在支援的 Hypervisor 上執行。 偵測到 VM 世代識別碼。<p>目前的 VM 世代識別碼值：%1 |
| **注意事項與解決方法** | 這是成功的事件，只有在發生未預期的狀況時才會成為問題。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2169** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | 未偵測到 VM 世代識別碼。 DC 可能架設於實體電腦或舊版的 Hyper-V 上，或架設於不支援 VM 世代識別碼的 Hypervisor 上。<p>其他資料<p>檢查 VM 世代識別碼時傳回的失敗碼：%1 |
| **注意事項與解決方法** | 如果不打算進行複製，這是成功的事件。 否則，請檢查 [系統] 事件記錄檔，並檢閱 Hypervisor 產品支援文件。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2170** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 警告 |
| **Message** | 偵測到世代識別碼變更。<p>在 DS 快取的世代識別碼 (舊值)：%1<p>VM 中目前的世代識別碼 (新值)：%2<p>在使用虛擬機器快照後、虛擬機器匯入作業後或即時移轉作業後會發生世代識別碼變更。 *<COMPUTERNAME>* 將建立新的調用識別碼來復原網域控制站。 虛擬的網域控制站不應使用虛擬機器快照來還原。 要還原或復原 Active Directory 網域服務資料庫的內容，支援的方法是還原以 Active Directory 網域服務感知備份應用程式建立的系統狀態備份。 |
| **注意事項與解決方法** | 如果想要進行複製，這是成功的事件。 否則，請檢查 [系統] 事件記錄檔。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2171** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | 未偵測到世代識別碼變更。<p>在 DS 快取的世代識別碼 (舊值)：%1<p>VM 中目前的世代識別碼 (新值)：%2 |
| **注意事項與解決方法** | 如果不打算進行複製，這是成功的事件，而且應該會在每次虛擬 DC 重新開機時顯示。 否則，請檢查 [系統] 事件記錄檔。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2172** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | 讀取網域控制站電腦物件的 msDS-GenerationId 屬性。<p>msDS-GenerationId 屬性值：%1 |
| **注意事項與解決方法** | 如果想要進行複製，這是成功的事件。 否則，請檢查 [系統] 事件記錄檔。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2173** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | 無法讀取網域控制站電腦物件的 msDS-GenerationId 屬性。 造成此問題的原因，可能是資料庫交易失敗或世代識別碼不存在本機資料庫。 msDS-GenerationId 在 dcpromo 後的第一次重新開機期間不存在，或 DC 不是虛擬網域控制站。<p>其他資料<p>失敗碼：%1 |
| **注意事項與解決方法** | 如果想要進行複製，而且是複製完成之後的第一次 VM 重新開機，則這是成功的事件。 在非虛擬的網域控制站上也可以忽略它。 否則，請檢查 [系統] 事件記錄檔。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2174** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | DC 不是虛擬網域控制站複製品，也不是還原的虛擬網域控制站快照。 |
| **注意事項與解決方法** | 如果不打算進行複製，這是成功的事件。 否則，請檢查 [系統] 事件記錄檔。 |

| 事件 | 描述 |
| -- |--|
|**事件識別碼**|**2175**|
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|
|**嚴重性**|錯誤|
|**Message**|虛擬網域控制站複製設定檔存在於不支援的平台。|
|**注意事項與解決方法**|這會在找到 dccloneconfig.xml 但找不到 VM 世代識別碼時發生，例如，在實體電腦上或在不支援 VM 世代識別碼的 Hypervisor 上找到 dccloneconfig.xml 檔案時。|

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2176** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | 已重新命名虛擬網域控制站複製品設定檔。<p>其他資料<p>舊檔案名稱：%1<p>新檔案名稱：%2 |
| **注意事項與解決方法** | 預期會在從來源 VM 備份開機時重新命名，因為 VM 世代識別碼尚未變更。 這樣可以防止來源網域控制站嘗試進行複製。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2177** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | 重新命名虛擬網域控制站複製設定檔失敗。<p>其他資料<p>檔案名稱：%1<p>失敗碼：%2 %3 |
| **注意事項與解決方法** | 預期會在從來源 VM 備份開機時嘗試重新命名，因為 VM 世代識別碼尚未變更。 這樣可以防止來源網域控制站嘗試進行複製。 手動重新命名檔案，並調查是否安裝了可能防止檔案重新命名的協力廠商產品。 |

| 事件 | 描述 |
| -- |--|
|**事件識別碼**|**2178**|
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|
|**嚴重性**|資訊|
|**Message**|偵測到虛擬網域控制站複製品設定檔，但 VM 世代識別碼未變更。 本機 DC 是複製品來源 DC。 請重新命名複製設定檔。|
|**注意事項與解決方法**|預期會在從來源 VM 備份開機時發生，因為 VM 世代識別碼尚未變更。 這樣可以防止來源網域控制站嘗試進行複製。|

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2179** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | 網域控制站電腦物件的 msDS-GenerationId 屬性已設為下列參數：<p>GenerationID 屬性：%1 |
| **注意事項與解決方法** | 這是成功的事件，只有在發生未預期的狀況時才會成為問題。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2180** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 警告 |
| **Message** | 無法設定網域控制站電腦物件的 msDS-GenerationId 屬性。<p>其他資料<p>失敗碼：%1 |
| **注意事項與解決方法** | 檢查 [系統] 事件記錄檔和 Dcpromo.log。 請查閱 MS TechNet、MS 知識庫及 MS 部落格中的特定錯誤，以判斷它的一般意義，然後根據這些結果來疑難排解。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2182** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | 內部事件：已要求目錄服務複製遠端 DSA： |
| **注意事項與解決方法** | 這是成功的事件，只有在發生未預期的狀況時才會成為問題。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2183** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | 內部事件： *<COMPUTERNAME>* 已完成複製遠端目錄服務代理程式的要求。<p>原始 DC 名稱：%3<p>要求複製 DC 名稱：%4<p>要求複製 DC 站台：%5<p>其他資料<p>錯誤值：%1 %2 |
| **注意事項與解決方法** | 這是成功的事件，只有在發生未預期的狀況時才會成為問題。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2184** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 無法為複製的 DC 建立網域控制站帳戶。<p>原始 DC 名稱：%1<p>允許的複製 DC 數目：%2<p>已超出可透過複製產生的網域控制站帳戶數目限制 <em> <COMPUTERNAME> </em> 。 |
| **注意事項與解決方法** | 根據命名慣例，如果未將網域控制站降級，單一來源網域控制站名稱只能自動產生 9999 次。 請在 XML 中使用 <computername> 元素來產生新的唯一名稱，或者從不同的具名 DC 中進行複製。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2191** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | *<COMPUTERNAME>* 設定下列登錄值以停用 DNS 更新。<p>登錄機碼：%1<p>登錄值：%2<p>登錄值資料：%3<p>複製程序期間，本機電腦與複製來源電腦可能短時間內會有相同的電腦名稱。 此期間內會停用 DNS A 與 AAAA 記錄登錄，因此用戶端無法傳送要求至正在進行複製的本機電腦。 完成複製以後，複製程序將再次啟用 DNS 更新。 |
| **注意事項與解決方法** | 這是成功的事件，只有在發生未預期的狀況時才會成為問題。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2192** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 無法設定下列登錄值以停用 DNS 更新。<p>登錄機碼：%1<p>登錄值：%2<p>登錄值資料：%3<p>錯誤碼：%4<p>錯誤訊息：%5<p>複製程序期間，本機電腦與複製來源電腦可能短時間內會有相同的電腦名稱。 此期間內會停用 DNS A 與 AAAA 記錄登錄，因此用戶端無法傳送要求至正在進行複製的本機電腦。 |
| **注意事項與解決方法** | 檢查 [應用程式] 和 [系統] 事件記錄檔。 調查可能封鎖登錄更新的協力廠商應用程式。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2193** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | *<COMPUTERNAME>* 設定下列登錄值以啟用 DNS 更新。<p>登錄機碼：%1<p>登錄值：%2<p>登錄值資料：%3<p>複製程序期間，本機電腦與複製來源電腦可能短時間內會有相同的電腦名稱。 此期間內會停用 DNS A 與 AAAA 記錄登錄，因此用戶端無法傳送要求至正在進行複製的本機電腦。 |
| **注意事項與解決方法** | 這是成功的事件，只有在發生未預期的狀況時才會成為問題。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2194** |
|--|--|
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 無法設定下列登錄值以啟用 DNS 更新。<p>登錄機碼：%1<p>登錄值：%2<p>登錄值資料：%3<p>錯誤碼：%4<p>錯誤訊息：%5<p>複製程序期間，本機電腦與複製來源電腦可能短時間內會有相同的電腦名稱。 此期間內會停用 DNS A 與 AAAA 記錄登錄，因此用戶端無法傳送要求至正在進行複製的本機電腦。 |
| **注意事項與解決方法** | 檢查 [應用程式] 和 [系統] 事件記錄檔。 調查可能封鎖登錄更新的協力廠商應用程式。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2195** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | 無法設定 DSRM 開機。<p>錯誤碼：%1<p>錯誤訊息：%2<p>當虛擬網域控制站複製失敗或虛擬網域控制站複製設定檔出現在不支援的 Hypervisor 上時，本機電腦將重新開機到 DSRM 以進行疑難排解。 設定 DSRM 開機失敗。 |
| **注意事項與解決方法** | 檢查 [應用程式] 和 [系統] 事件記錄檔。 調查可能封鎖登錄更新的協力廠商應用程式。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2196** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | 無法啟用關機權限。<p>錯誤碼：%1<p>錯誤訊息：%2<p>當虛擬網域控制站複製失敗或虛擬網域控制站複製設定檔出現在不支援的 Hypervisor 上時，本機電腦將重新開機到 DSRM 以進行疑難排解。 啟用關機權限失敗。 |
| **注意事項與解決方法** | 檢查 [應用程式] 和 [系統] 事件記錄檔。 調查可能封鎖權限使用方式的協力廠商應用程式。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2197** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | 無法起始系統關機。<p>錯誤碼：%1<p>錯誤訊息：%2<p>當虛擬網域控制站複製失敗或虛擬網域控制站複製設定檔出現在不支援的 Hypervisor 上時，本機電腦將重新開機到 DSRM 以進行疑難排解。 起始系統關機失敗。 |
| **注意事項與解決方法** | 檢查 [應用程式] 和 [系統] 事件記錄檔。 調查可能封鎖權限使用方式的協力廠商應用程式。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2198** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 無法建立或修改下列複製的 DC 物件。<p>其他資料：<p>物件：<p>%1<p>錯誤值：%2<p>%3 |
| **注意事項與解決方法** | 請查閱 MS TechNet、MS 知識庫及 MS 部落格中的特定錯誤，以判斷它的一般意義，然後根據這些結果來疑難排解。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2199** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 無法建立下列複製的 DC 物件，因為該物件已存在。<p>其他資料：<p>來源 DC：<p>%1<p>物件：<p>%2 |
| **注意事項與解決方法** | 驗證 dccloneconfig.xml 未指定現有的網域控制站，或者在未編輯名稱的情況下，於多個複製品上使用 dccloneconfig.xml 的複本。 如果仍發生未預期的衝突，請判斷是哪位系統管理員將它升級的；與他們連絡，以討論是否應將現有的網域控制站降級、清除現有的網域控制站中繼資料，或者複製品是否應該使用不同的名稱。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2203** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | 上一個虛擬網域控制站複製作業失敗。 這是上次複製作業後第一次重新開機，因此這應該是複製重試。 但是，虛擬網域控制站複製設定檔不存在，也未偵測到虛擬機器世代識別碼變更。 開機進入 DSRM。<p>上一個虛擬網域控制站複製作業失敗：%1<p>虛擬網域控制站複製設定檔存在：%2<p>偵測到虛擬機器產生識別碼變更：%3 |
| **注意事項與解決方法** | 預期會在先前的複製失敗時發生，因為 dccloneconfig.xml 遺失或無效 |

| 事件 | 描述 |
|--|--|
| 事件識別碼 | 2210 |
| 來源 | Microsoft-Windows-ActiveDirectory_DomainService |
| Severity | 錯誤 |
| 訊息 | <COMPUTERNAME> 無法為複製網域控制站建立物件。<p>其他資料：<p>複製識別碼：%6<p>複製網域控制站名稱：%1<p>重試迴圈：%2<p>例外狀況值：%3<p>錯誤值：%4<p>DSID：%5 |
| 注意事項與解決方法 | 檢閱 [系統] 與 [目錄服務] 事件記錄檔及 dcpromo.log，進一步取得複製為何失敗的詳細資料。 |

| 事件 | 描述 |
|--|--|
| 事件識別碼 | 2211 |
| 來源 | Microsoft-Windows-ActiveDirectory_DomainService |
| Severity | 資訊 |
| 訊息 | <COMPUTERNAME> 已為複製網域控制站建立物件。<p>其他資料：<p>複製識別碼：%3<p>複製網域控制站名稱：%1<p>重試迴圈：%2 |
| 注意事項與解決方法 | 這是成功的事件，只有在發生未預期的狀況時才會成為問題。 |

| 事件 | 描述 |
|--|--|
| 事件識別碼 | 2212 |
| 來源 | Microsoft-Windows-ActiveDirectory_DomainService |
| Severity | 資訊 |
| 訊息 | <COMPUTERNAME> 已開始為複製網域控制站建立物件。<p>其他資料：<p>複製識別碼：%1<p>複製名稱：%2<p>複製站台：%3<p>複製 RODC：%4 |
| 注意事項與解決方法 | 這是成功的事件，只有在發生未預期的狀況時才會成為問題。 |

| 事件 | 描述 |
|--|--|
| 事件識別碼 | 2213 |
| 來源 | Microsoft-Windows-ActiveDirectory_DomainService |
| Severity | 資訊 |
| 訊息 | <COMPUTERNAME> 已為唯讀網域控制站複製建立新的 KrbTgt 物件。<p>其他資料：<p>複製識別碼：%1<p>新的 KrbTgt 物件 Guid：%2 |
| 注意事項與解決方法 | 這是成功的事件，只有在發生未預期的狀況時才會成為問題。 |

| 事件 | 描述 |
| -- |--|
|事件識別碼|2214|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|資訊|
|訊息|<COMPUTERNAME> 將為複製網域控制站建立電腦物件。<p>其他資料：<p>複製識別碼：%1<p>原始網域控制站：%2<p>複製網域控制站：%3|
|注意事項與解決方法|這是成功的事件，只有在發生未預期的狀況時才會成為問題。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2215|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|資訊|
|訊息|<COMPUTERNAME> 將在下列站台新增複製網域控制站。<p>其他資料：<p>複製識別碼：%1<p>站台：%2|
|注意事項與解決方法|這是成功的事件，只有在發生未預期的狀況時才會成為問題。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2216|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|資訊|
|訊息|<COMPUTERNAME> 將為複製網域控制站建立伺服器容器。<p>其他資料：<p>複製識別碼：%1<p>伺服器容器：%2|
|注意事項與解決方法|這是成功的事件，只有在發生未預期的狀況時才會成為問題。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2217|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|資訊|
|訊息|<COMPUTERNAME> 將為複製網域控制站建立伺服器物件。<p>其他資料：<p>複製識別碼：%1<p>伺服器物件：%2|
|注意事項與解決方法|這是成功的事件，只有在發生未預期的狀況時才會成為問題。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2218|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|資訊|
|訊息|<COMPUTERNAME> 將為複製網域控制站建立 NTDS 設定物件。<p>其他資料：<p>複製識別碼：%1<p>物件：%2|
|注意事項與解決方法|這是成功的事件，只有在發生未預期的狀況時才會成為問題。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2219|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|資訊|
|訊息|<COMPUTERNAME> 將為複製唯讀網域控制站建立連線物件。<p>其他資料：<p>複製識別碼：%1|
|注意事項與解決方法|這是成功的事件，只有在發生未預期的狀況時才會成為問題。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2220|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|資訊|
|訊息|<COMPUTERNAME> 將為複製唯讀網域控制站建立 SYSVOL 物件。<p>其他資料：<p>複製識別碼：%1|
|注意事項與解決方法|這是成功的事件，只有在發生未預期的狀況時才會成為問題。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2221|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|錯誤|
|訊息|<COMPUTERNAME> 無法為複製的網域控制站產生隨機密碼。<p>其他資料：<p>複製識別碼：%1<p>複製網域控制站名稱：%2<p>錯誤：%3 %4|
|注意事項與解決方法|檢查 [系統] 事件記錄檔，進一步取得為何無法建立電腦帳戶密碼的詳細資料。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2222|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|錯誤|
|訊息|<COMPUTERNAME> 無法為複製的網域控制站設定密碼。<p>其他資料：<p>複製識別碼：%1<p>複製網域控制站名稱：%2<p>錯誤：%3 %4|
|注意事項與解決方法|檢查 [系統] 事件記錄檔，進一步取得為何無法設定電腦帳戶密碼的詳細資料。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2223|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|資訊|
|訊息|<COMPUTERNAME> 已順利為複製的網域控制站設定電腦帳戶密碼。<p>其他資料：<p>複製識別碼：%1<p>複製網域控制站名稱：%2<p>總嘗試次數：%3|
|注意事項與解決方法|這是成功的事件，只有在發生未預期的狀況時才會成為問題。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2224|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|錯誤|
|訊息|虛擬網域控制站複製失敗。 下列 %1 受管理的服務帳戶存在於複製的電腦上：<p>%2<p>若要順利複製，必須移除所有受管理的服務帳戶。 您可以使用 Remove-ADComputerServiceAccount PowerShell Cmdlet 來完成此動作。|
|注意事項與解決方法|預期會在使用獨立的 MSA (非群組 MSA) 時發生。 請勿  依照事件建議來移除帳戶 - 它的內容不正確。 使用 Uninstall-AdServiceAccount- [https://technet.microsoft.com/library/hh852310](/previous-versions/windows/it-pro/windows-powershell-1.0/ee176927(v=technet.10)) 。<p>獨立的 MSA (在 Windows Server 2008 R2 中首次發行) 已在 Windows Server 2012 中使用群組 MSA (gMSA) 來取代。 GMSA 支援複製。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2225|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|資訊|
|訊息|已順利將下列安全性主體的已快取密碼從本機網域控制站移除：<p>%1<p>複製唯讀網域控制站之後，系統會將先前在複製來源唯讀網域控制站上快取的密碼從複製的網域控制站移除。|
|注意事項與解決方法|這是成功的事件，只有在發生未預期的狀況時才會成為問題。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2226|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|錯誤|
|訊息|無法將下列安全性主體的已快取密碼從本機網域控制站移除：<p>%1<p>錯誤：%2 (%3)<p>複製唯讀網域控制站之後，必須在複製品上移除先前在複製來源唯讀網域控制站上快取的密碼，以降低攻擊者可從竊取或遭破解之複製品取得這些認證的風險。 若安全性主體是具有極高權限的帳戶而且應該受保護以免於上述情況，請使用 rootDSE 作業 rODCPurgeAccount 在本機網域控制站上手動清除其密碼。|
|注意事項與解決方法|檢查 [系統] 和 [目錄服務] 事件記錄檔，以取得進一步資訊。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2227|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|錯誤|
|訊息|嘗試將已快取密碼從本機網域控制站移除時發生例外狀況。<p>其他資料：<p>例外狀況值：%1<p>錯誤值：%2<p>DSID：%3<p>複製唯讀網域控制站之後，必須在複製品上移除先前在複製來源唯讀網域控制站上快取的密碼，以降低攻擊者可從竊取或遭破解之複製品取得這些認證的風險。 若這其中任一個安全性主體是具有極高權限的帳戶而且應該受保護以免於上述情況，請使用 rootDSE 作業 rODCPurgeAccount 在本機網域控制站上手動清除其密碼。|
|注意事項與解決方法|檢查 [系統] 和 [目錄服務] 事件記錄檔，以取得進一步資訊。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|2228|
|來源|Microsoft-Windows-ActiveDirectory_DomainService|
|Severity|錯誤|
|訊息|此網域控制站之 Active Directory 資料庫中的虛擬機器世代識別碼與此虛擬機器目前的值不同。 但是，找不到虛擬網域控制站複製設定檔 (DCCloneConfig.xml)，因此無法嘗試網域控制站複製。 若您是要執行網域控制站複製作業，請確定已在任一個支援的位置提供 DCCloneConfig.xml。 此外，此網域控制站的 IP 位址與另一個網域控制站的 IP 位址衝突。 為確保服務不中斷，已將網站控制站設定為開機到 DSRM。<p>其他資料：<p>重複的 IP 位址：%1|
|注意事項與解決方法|這個保護機制會視需要停止重複的網域控制站 (例如，使用 DHCP 時便不會停止)。 新增有效的 DcCloneConfig.xml 檔案、移除 DSRM 旗標，以及重新嘗試複製|

| 事件 | 描述 |
| -- |--|
|事件識別碼|29218|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|錯誤|
|訊息|虛擬網域控制站複製失敗。 複製作業無法完成，且複製的網域控制站已重新開機至目錄服務還原模式 (DSRM)。<p>請檢查先前記錄的事件與 %systemroot%\debug\dcpromo.log，以取得有關虛擬網域控制站複製嘗試之錯誤以及此複製映像是否可重複使用的詳細資訊。<p>若有一或多個記錄項目指出無法重試複製程序，則必須安全地刪除此映像。 否則，您可以修正錯誤、清除 DSRM 開機旗標，然後重新開機到標準模式。在重新開機時，將重試複製作業。|
|注意事項與解決方法|檢閱 [系統] 與 [目錄服務] 事件記錄檔及 dcpromo.log，進一步取得複製為何失敗的詳細資料。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|29219|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|資訊|
|訊息|虛擬網域控制站複製成功。|
|注意事項與解決方法|這是成功的事件，只有在發生未預期的狀況時才會成為問題。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|29248|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|錯誤|
|訊息|虛擬網域控制站複製無法取得 Winlogon 通知。 傳回的錯誤碼是 %1 (%2)。<p>如需有關此錯誤的詳細資訊，請檢閱 %systemroot%\debug\dcpromo.log，以了解對應至虛擬網域控制站複製嘗試的錯誤。|
|注意事項與解決方法|請連絡 Microsoft 產品支援|

| 事件 | 描述 |
| -- |--|
|事件識別碼|29249|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|錯誤|
|訊息|虛擬網域控制站複製無法剖析虛擬網域控制站設定檔。<p>傳回的 HRESULT 代碼是 %1。<p>設定檔是：%2<p>請修正設定檔中的錯誤，然後嘗試重新執行複製作業。<p>如需有關此錯誤的詳細資訊，請參閱 %systemroot%\debug\dcpromo.log。|
|注意事項與解決方法|使用 XML 編輯器檢查 dclconeconfig.xml 檔案以取得語法錯誤，並檢查 DCCloneConfigSchema.xsd 結構描述檔案。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|29250|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|錯誤|
|訊息|虛擬網域控制站複製失敗。 已複製的虛擬網域控制站上目前有已啟用的軟體或服務，而它們未出現在虛擬網域控制站複製允許的應用程式清單中。<p>下列是缺少的項目：<p>%2<p>%1 (如果有的話) 已使用為已定義的包含清單。<p>若未安裝可複製的應用程式，將無法完成複製作業。<p>請執行 Active Directory PowerShell Cmdlet Get-ADDCCloningExcludedApplicationList，以檢查已複製的電腦上已安裝但未包含在允許清單中的應用程式，然後將它們新增到允許清單 (若它們與虛擬網域控制站複製相容)。 若這些應用程式中有任一個應用程式與虛擬網域控制站複製不相容，請先將它們解除安裝，然後再嘗試重新執行複製作業。<p>虛擬網域控制站複製程序會根據下列搜尋順序來搜尋允許的應用程式清單檔案 CustomDCCloneAllowList.xml，而且會使用找到的第一個檔案並忽略其他檔案：<p>1. 登錄值名稱： HKey_Local_Machine \System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<p>2. DSA 工作目錄資料夾所在的相同目錄<p>3. %windir%\NTDS<p>4. 可移動讀取/寫入媒體（依磁碟機號位於磁片磁碟機根目錄的順序）|
|注意事項與解決方法|依照訊息指示執行|

| 事件 | 描述 |
|--|--|
| 事件識別碼 | 29251 |
| 來源 | Microsoft-Windows-DirectoryServices-DSROLE-Server |
| Severity | 錯誤 |
| 訊息 | 虛擬網域控制站複製無法重設複製電腦的 IP 位址。<p>傳回的錯誤碼是 %1 (%2)。<p>此錯誤可能是虛擬網域控制站設定檔之網路設定區段中的錯誤設定所造成。<p>請參閱 %systemroot%\debug\dcpromo.log，以了解對應至虛擬網域控制站複製嘗試期間之 IP 位址重設錯誤的詳細資訊。<p>如需重設複製電腦上電腦 IP 位址的詳細資訊，請參閱： https://go.microsoft.com/fwlink/?LinkId=208030 |
| 注意事項與解決方法 | 確定 dccloneconfig.xml 中設定的 IP 資訊是有效的，且不會複製原始來源電腦。 |

| 事件 | 描述 |
|--|--|
| 事件識別碼 | 29253 |
| 來源 | Microsoft-Windows-DirectoryServices-DSROLE-Server |
| Severity | 錯誤 |
| 訊息 | 虛擬網域控制站複製失敗。 複製網域控制站無法在複製之機器的複製之電腦主網域中找到網域主控站 (PDC) 操作主機。<p>傳回的錯誤碼是 %1 (%2)。<p>請確定複製之機器主網域中的網域主控站已指派至運作中的網域控制站，而且已上線並正常運作。 確定複製的機器可以使用要求的連接埠與通訊協定透過 LDAP/RPC 連線到網域主控站。 |
| 注意事項與解決方法 | 驗證已設定複製的網域控制站 IP 和 DNS 資訊。 使用 Dcdiag.exe/test： locatorcheck 來驗證 PDCE 是否已上線、使用 Nltest.exe/server： *<PDCE>* /dclist： *<domain>* 有效的 RPC、在複製失敗時從 PDCE 取得網路抓取，並分析流量。 |

| 事件 | 描述 |
|--|--|
| 事件識別碼 | 29254 |
| 來源 | Microsoft-Windows-DirectoryServices-DSROLE-Server |
| Severity | 錯誤 |
| 訊息 | 虛擬網域控制站複製無法繫結至網域主控站 %1。<p>傳回的錯誤碼是 %2 (%3)。<p>請確定網域主控站 %1 已上線並正常運作。 確定複製的機器可以使用要求的連接埠與通訊協定透過 LDAP/RPC 連線到網域主控站。 |
| 注意事項與解決方法 | 驗證已設定複製的網域控制站 IP 和 DNS 資訊。 使用 Dcdiag.exe/test： locatorcheck 來驗證 PDCE 是否已上線、使用 Nltest.exe/server： *<PDCE>* /dclist： *<domain>* 有效的 RPC、在複製失敗時從 PDCE 取得網路抓取，並分析流量。 |

| 事件 | 描述 |
| -- |--|
|事件識別碼|29255|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|錯誤|
|訊息|虛擬網域控制站複製失敗。<p>因為要複製的映像要求而嘗試在網域主控站 %1 上建立物件的動作傳回錯誤 %2 (%3)。<p>請確認複製的網域控制站有權限可以複製本身。 檢查網域主控站 %1 上 [目錄服務] 事件記錄檔中的相關事件。|
|注意事項與解決方法|請查閱 MS TechNet、MS 知識庫及 MS 部落格中的特定錯誤，以判斷它的一般意義，然後根據這些結果來疑難排解。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|29256|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|錯誤|
|訊息|嘗試設定 [開機到目錄服務還原模式] 旗標失敗，錯誤碼為 %1。<p>如需有關錯誤的詳細資訊，請參閱 %systemroot%\debug\dcpromo.log。|
|注意事項與解決方法|檢查 [目錄服務] 記錄檔和 dcpromo.log，以取得詳細資訊。 檢查 [應用程式] 和 [系統] 事件記錄檔。 調查可能封鎖權限使用方式的協力廠商應用程式。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|29257|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|錯誤|
|訊息|虛擬網域控制站複製已完成。 嘗試將虛擬機器重新開機失敗，錯誤碼為 %1。<p>請將該機器重新開機，以完成複製作業。|
|注意事項與解決方法|檢查 [應用程式] 和 [系統] 事件記錄檔。 調查可能封鎖權限使用方式的協力廠商應用程式。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|29264|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|錯誤|
|訊息|嘗試清除 [開機到目錄服務還原模式] 旗標失敗，錯誤碼為 %1。<p>如需有關錯誤的詳細資訊，請參閱 %systemroot%\debug\dcpromo.log。|
|注意事項與解決方法|檢查 [目錄服務] 記錄檔和 dcpromo.log，以取得詳細資訊。 檢查 [應用程式] 和 [系統] 事件記錄檔。 調查可能封鎖權限使用方式的協力廠商應用程式。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|29265|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|資訊|
|訊息|虛擬網域控制站複製成功。 虛擬網域控制站複製設定檔 %1 已重新命名為 %2。|
|注意事項與解決方法|N/A，這是成功的事件。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|29266|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|錯誤|
|訊息|虛擬網域控制站複製成功。 嘗試重新命名虛擬網域控制站複製設定檔 %1 失敗，錯誤碼為 %2 (%3)。|
|注意事項與解決方法|手動重新命名 dccloneconfig.xml 檔案。|

| 事件 | 描述 |
| -- |--|
|事件識別碼|29267|
|來源|Microsoft-Windows-DirectoryServices-DSROLE-Server|
|Severity|錯誤|
|訊息|虛擬網域控制站複製無法檢查虛擬網域控制站複製允許的應用程式清單。<p>傳回的錯誤碼是 %1 (%2)。<p>此錯誤可能是複製允許清單檔案 (目前要檢查的檔案是：%3)。 如需有關此錯誤的詳細資訊，請參閱 %systemroot%\debug\dcpromo.log。|
|注意事項與解決方法|依照事件指示執行|

##### <a name="error-messages"></a>錯誤訊息
針對失敗的虛擬網域控制站複製沒有任何直接的互動式錯誤；所有的複製資訊都記錄於 [系統] 和 [目錄服務] 記錄檔中，且網域控制站升級記錄於 dcpromo.log 中。 但是，如果伺服器開機進入 DS 還原模式，請在升級或複製失敗時立即進行調查。

dcpromo.log 是第一個檢查複製失敗的地方。 根據列出的失敗而定，後續可能需要檢閱 [目錄服務] 和 [系統] 記錄檔，進一步進行診斷。

#### <a name="known-issues-and-support-scenarios"></a>已知的問題和支援案例
以下是 Windows Server 2012 開發程序的過程中常見的問題。 這所有的問題都是「經過設計的」，具有有效的因應措施或更適當的技術，可在第一時間避免發生這些問題。 有些問題可能會在 Windows Server 2012 的較新版本中解決。

|**問題**|**複製失敗，DSRM**|
| -- |--|
|**徵兆**|複製品開機進入目錄服務還原模式|
|**解決方式和注意事項**|驗證緊接在＜部署虛擬網域控制站＞一節和[適用於疑難排解虛擬網域控制站複製功能的一般方法](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)之後的所有步驟<p>說明請見 KB 2742844。|

|**問題**|**使用 DHCP 進行複製時要租用額外的 IP**|
| -- |--|
|**徵兆**|順利複製 DC 並使用 DHCP 之後，當複製品第一次開機時會取得 DHCP 租用。 接著，在將伺服器重新命名並重新啟動為 DC 時，它會取得第二個 DHCP 租用。 第一個 IP 位址並未釋出，而將成為「虛設」租用|
|**解決方式和注意事項**|手動刪除 DHCP 中未使用的位址租用，或允許它以正常方式到期。 說明請見 KB 2742836。|

|**問題**|**在非常長的延遲之後複製失敗進入 DSRM**|
| -- |--|
|**徵兆**|複製會在 [網域控制站複製已完成 X%] 時出現暫停狀態 8 到 15 分鐘。 在此之後，複製會失敗並開機進入 DSRM。|
|**解決方式和注意事項**|複製的電腦無法從 DHCP 或 SLAAC 取得動態 IP 位址、使用重複的 IP 位址，或者找不到 PDC。 由複製所執行的多個重試嘗試會導致延遲。 請解決網路問題以允許複製。<p>說明請見 KB 2742844。|

|**問題**|**複製並未重新建立所有服務主體名稱**|
| -- |--|
|**徵兆**|如果有一組含「三部分」  的服務主體名稱 (SPN) 同時包含一個含有連接埠的 NetBIOS 名稱，以及一個幾乎完全相同但不含連接埠的 NetBIOS 名稱，就不會以新的電腦名稱重新建立沒有連接埠的項目。 例如：<p>customspn/DC1:200/app1 INVALID USE OF SYMBOLS 這會使用新的電腦名稱重新建立 <p>customspn/DC1/app1 INVALID USE OF SYMBOLS 這不會使用新的電腦名稱重新建立 <p>不論是否有連接埠，皆會重新建立完整名稱與不含三部分的 SPN。 例如，會在複製品上順利重新建立下列項目：<p>customspn/DC1:202 INVALID USE OF SYMBOLS 這會重新建立 <p>customspn/DC1 INVALID USE OF SYMBOLS 這會重新建立 <p>customspn/DC1.corp.contoso.com:202 INVALID USE OF SYMBOLS 這是重新建立的名稱 <p>customspn/DC1.corp.contoso.com INVALID USE OF SYMBOLS 這會重新建立 |
|**解決方式和注意事項**|這是 Windows 中網域控制站重新命名程序的限制，而不只是複製中的限制。 在任何案例中，重新命名邏輯都不會處理含三部分的 SPN。 大多數 (包括 Windows 服務) 都不會受到此項所影響，因為它們會視需要建立任何遺失的 SPN。 其他應用程式可能需要手動輸入 SPN 來解決問題。<p>說明請見 KB 2742874。|

|**問題**|**複製失敗、開機進入 DSRM、一般網路錯誤**|
| -- |--|
|**徵兆**|複製品開機進入目錄服務修復模式。 發生一般網路錯誤。|
|解決方式和注意事項|確定新的複製品不含從來源網域控制站指派的重複靜態 MAC 位址；您可以針對來源和複製虛擬機器在 Hypervisor 主機上執行這個命令，查看 VM 是否使用靜態 MAC 位址：<p>VMName *測試-* vm &#124; Get-VMNetworkAdapter &#124; fl *<p>將 MAC 位址變更為唯一的靜態位址，或者切換為使用動態 MAC 位址。<p>說明請見 KB 2742844|

| **問題** | **複製失敗，開機進入 DSRM 以做為來源 DC 的複本** |
| -- |--|
| **徵兆** | 新的複製品會在未進行複製的情況下開機。 dccloneconfig.xml 不會重新命名，而且會在 DS 還原模式中啟動伺服器。 [目錄服務] 事件記錄檔顯示錯誤 2164<p>*<COMPUTERNAME>* 無法啟動 DsRoleSvc 服務，以複製本機虛擬網域控制站。 |
| **解決方式和注意事項** | 檢查「DS 角色伺服器」服務 (DsRoleSvc) 的服務設定，並確定已將它的啟動類型設為 [手動]。 請確定沒有任何協力廠商程式會防止這個服務啟動。<p>如需如何回收此次要 DC 同時確保更新可輸出複寫的詳細資訊，請參閱 Microsoft 知識庫文章 2742970。 |

|**問題**|**複製失敗、開機進入 DSRM、錯誤 8610**|
| -- |--|
|**徵兆**|複製品開機進入目錄服務還原模式。 Dcpromo .log 顯示 8610 錯誤 (此為 ERROR_DS_ROLE_NOT_VERIFIED 8610 或 0x21A2)|
|**解決方式和注意事項**|如果 PDC 是可搜尋的但尚未執行足夠的複寫以允許本身擔任該角色時，將會發生此問題。 例如，如果已開始複製但另一位系統管理員將 PDCE FSMO 角色移至新的 DC。<p>說明請見 KB 2742916。|

|**問題**|**複製失敗、開機進入 DSRM、一般網路錯誤**|
| -- |--|
|**徵兆**|複製品開機進入目錄服務還原模式。 發生一般網路錯誤。|
|**解決方式和注意事項**|確定新的複製品不含從來源網域控制站指派的重複靜態 MAC 位址；您可以針對來源和複製虛擬機器在 Hyper-V 主機上執行這個命令，查看 VM 是否使用靜態 MAC 位址：<p>VMName *測試-* vm &#124; Get-VMNetworkAdapter &#124; fl *<p>將 MAC 位址變更為唯一的靜態位址，或者切換為使用動態 MAC 位址。<p>說明請見 KB 2742844。|

|**問題**|**複製失敗、開機進入 DSRM**|
| -- |--|
|**徵兆**|複製品開機進入目錄服務修復模式|
|**解決方式和注意事項**|確保 dccloneconfig.xml 包含結構描述定義 (請參閱 sampledccloneconfig.xml，第 2 行)：<p>**<d3c： DCCloneConfig xmlns:d3c = "uri: .com：架構： DCCloneConfig" >**<p>說明請見 KB 2742844|

|**問題**|**沒有登入伺服器可用來為進入 DSRM 記錄錯誤**|
| -- |--|
|**徵兆**|複製品開機進入目錄服務修復模式。 您嘗試登入並收到錯誤：<p>**目前沒有任何登入伺服器可用來服務登入要求**|
|**解決方式和注意事項**|確定您是以 DSRM 系統管理員帳戶登入，而不是以網域帳戶登入。 使用向左鍵並輸入下列使用者名稱：<p>**.\administrator**<p>說明請見 KB 2742908|

|**問題**|**複製來源無法進入 DSRM，發生錯誤**|
|--|--|
|**徵兆**|複製期間發生失敗 8437「在 PDC 上建立複製 DC 物件失敗」(0x20f5)|
|**解決方式和注意事項**|在 DCCloneConfig.xml 中設定與來源 DC 或現有 DC 重複的電腦名稱。 電腦名稱也需要具備 NetBIOS 電腦名稱格式 (15 個或以下的字元，非 FQDN)。<p>設定唯一的有效名稱來修正 dccloneconfig.xml 檔案。<p>說明請見 KB 2742959|

|**問題**|**New-addccloneconfigfile 錯誤「索引超過範圍」**|
|--|--|
|**徵兆**|執行 new-addccloneconfigfile Cmdlet 時，收到錯誤訊息：<p>索引超過範圍。 必須為非負數且小於集合的大小。|
|**解決方式和注意事項**|您必須以提升權限的系統管理員身分，在 Windows PowerShell 主控台中執行這個 Cmdlet。 這個錯誤是因為電腦上缺少本機系統管理員群組成員資格所導致。<p>說明請見 KB 2742927|

|**問題**|**複製失敗，重複的 DC**|
|--|--|
|**徵兆**|複製品會在未執行複製的情況下開機，複製現有的來源 DC|
|**解決方式和注意事項**|電腦已複製且啟動，但是未在任一個支援的位置中包含 DcCloneConfig.xml 檔案，而且沒有與來源網域控制站重複的 IP 位址。 DC 必須正確移除，才能避免資料遺失。<p>說明請見 KB 2742970|

|**問題**|**New-ADDCCloneConfigFile 檢查當 GC 無法使用時，來源網域控制站是否為 [可複製的網域控制站] 群組的成員時失敗，且發生「無法操作伺服器」的錯誤。**|
|--|--|
|**徵兆**|執行 New-ADDCCloneConfigFile 以建立 dccloneconfig.xml 檔案時，收到錯誤訊息：<p>程式碼-伺服器無法運作|
|**解決方式和注意事項**|確認從您執行 New-ADDCCloneConfigFile 的伺服器到 GC 的連線，並確認已將 [可複製的網域控制站] 群組中來源網域控制站的成員資格複寫到該 GC。<p>針對最近可能已將 GC 或 DC 離線的案例，執行下列命令以做為排清 DC 定位程式快取的方法：<p>程式碼-nltest/dsgetdc：/GC/FORCE|

### <a name="advanced-troubleshooting"></a>進階疑難排解
這個單元會使用「處理中」  的記錄檔做為範例，並說明發生的內容，以教導使用者進行進階疑難排解。 如果您了解什麼是成功的虛擬化網域控制站作業，便很容易察覺環境中的失敗。 這些記錄檔是以來源呈現，會以每個記錄檔中複製網域控制站相關的「預期」  事件的遞增順序來顯示 (即使它們是警告和錯誤)。

#### <a name="cloning-a-domain-controller"></a>複製網域控制站
在這個範例中，複製網域控制站會使用 DHCP 來取得 IP 位址、使用 FRS 或 DFSR 複寫 SYSVOL (請視需要參閱適當的記錄檔)、做為通用類別目錄，以及使用空白的 dccloneconfig.xml 檔案。

##### <a name="directory-services-event-log"></a>目錄服務事件記錄檔
[目錄服務] 記錄檔包含大部分以事件為基礎的複製作業資訊。 Hypervisor 會變更 VM 世代識別碼，而 NTDS 服務會記錄它，接著使 RID 集區失效，並變更呼叫識別碼。 已設定新的 VM 世代識別碼，而伺服器會輸入複寫 Active Directory 資料。 DFSR 服務已停止，並已刪除裝載 SYSVOL 的資料庫，強制執行輸入的非權威同步處理。 已調整 USN 上限標準。

| **事件識別碼** | **Source** | **Message** |
|--|--|--|
| **2160** | ActiveDirectory_DomainService | 本機 Active Directory 網域服務發現虛擬網域控制站複製設定檔。<p>發現的虛擬網域控制站複製設定檔位於：<p>*<path>* \DCCloneConfig.xml<p>虛擬網域控制站複製設定檔的存在，表示本機虛擬網域控制站是另一個虛擬網域控制站的複製品。 Active Directory 網域服務將開始自行複製。 |
| **2191** | ActiveDirectory_DomainService | Active Directory 網域服務會設定下列登錄值以停用 DNS 更新。<p>登錄機碼：<p>SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<p>登錄值：<p>UseDynamicDns<p>登錄值資料：<p>0<p>複製程序期間，本機電腦與複製來源電腦可能短時間內會有相同的電腦名稱。 此期間內會停用 DNS A 與 AAAA 記錄登錄，因此用戶端無法傳送要求至正在進行複製的本機電腦。 完成複製以後，複製程序將再次啟用 DNS 更新。 |
| **2191** | ActiveDirectory_DomainService | Active Directory 網域服務會設定下列登錄值以停用 DNS 更新。<p>登錄機碼：<p>SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<p>登錄值：<p>RegistrationEnabled<p>登錄值資料：<p>0<p>複製程序期間，本機電腦與複製來源電腦可能短時間內會有相同的電腦名稱。 此期間內會停用 DNS A 與 AAAA 記錄登錄，因此用戶端無法傳送要求至正在進行複製的本機電腦。 完成複製以後，複製程序將再次啟用 DNS 更新。<p>"Information 2/7/2012 3:12:49 PM Microsoft-Windows-ActiveDirectory_DomainService 2191 Internal Configuration" Active Directory Domain Services 設定下列登錄值以停用 DNS 更新。<p>登錄機碼：<p>SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<p>登錄值：<p>DisableDynamicUpdate<p>登錄值資料：<p>1<p>複製程序期間，本機電腦與複製來源電腦可能短時間內會有相同的電腦名稱。 此期間內會停用 DNS A 與 AAAA 記錄登錄，因此用戶端無法傳送要求至正在進行複製的本機電腦。 完成複製以後，複製程序將再次啟用 DNS 更新。 |
| **2172** | ActiveDirectory_DomainService | 讀取網域控制站電腦物件的 msDS-GenerationId 屬性。<p>msDS GenerationId 屬性值：<p>*<Number>* |
| **2170** | ActiveDirectory_DomainService | 偵測到世代識別碼變更。<p>在 DS 快取的世代識別碼 (舊值)：<p>*<Number>*<p>VM 中目前的世代識別碼 (新值)：<p>*<Number>*<p>在使用虛擬機器快照後、虛擬機器匯入作業後或即時移轉作業後會發生世代識別碼變更。 Active Directory 網域服務將建立新的呼叫識別碼以復原網域控制站。 虛擬的網域控制站不應使用虛擬機器快照來還原。 要還原或復原 Active Directory 網域服務資料庫的內容，支援的方法是還原以 Active Directory 網域服務感知備份應用程式建立的系統狀態備份。 |
| **1109** | ActiveDirectory_DomainService | 這個目錄伺服器的 invocationID 屬性已變更。 在建立備份時最高的更新序號如下：<p>InvocationID 屬性 (舊值)：<p>*<GUID>*<p>InvocationID 屬性 (新值)：<p>*<GUID>*<p>更新序號：<p>*<Number>*<p>當目錄伺服器從備份媒體還原、設定為裝載可寫入的應用程式目錄分割，或在套用虛擬機器快照後、虛擬機器匯入作業後或即時移轉作業後恢復執行時，invocationID 即會變更。 虛擬的網域控制站不應使用虛擬機器快照來還原。 要還原或復原 Active Directory 網域服務資料庫的內容，支援的方法是還原以 Active Directory 網域服務感知備份應用程式建立的系統狀態備份。 |
| **1000** | ActiveDirectory_DomainService | Microsoft Active Directory 網域服務啟動完成。 |
| **1394** | ActiveDirectory_DomainService | 已經清除所有妨礙更新至 Active Directory 網域服務資料庫的問題。 已經成功更新 Active Directory 網域服務資料庫。 Net Logon 服務已經重新啟動 |
| **2163** | ActiveDirectory_DomainService | 已啟動 DsRoleSvc 服務以複製本機虛擬網域控制站。 |
| **326** | NTDS ISAM | NTDS (536) NTDSA： database engine 附加資料庫 (1，C:\Windows\NTDS\ntds.dit) 。 (時間=0 秒)<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.016、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.000、[12] 0.000。<p>已儲存快取：1 |
| **103** | NTDS ISAM | NTDS (536) NTDSA：資料庫引擎已停止實例 (0) 。<p>中途關機：0<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.032、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.031、[10] 0.000、[11] 0.000、[12] 0.000、[13] 0.000、[14] 0.000、[15] 0.000。 |
| **102** | NTDS ISAM | NTDS (536) NTDSA： database engine (6.02.8225.0000) 正在啟動新的實例 (0) 。 |
| **105** | NTDS ISAM | NTDS (536) NTDSA：資料庫引擎已啟動新的實例 (0) 。 (時間=0 秒)<p>內部計時順序：[1] 0.016、[2] 0.000、[3] 0.015、[4] 0.078、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.046、[10] 0.000、[11] 0.000。 |
| **1004** | ActiveDirectory_DomainService | Active Directory 網域服務成功地關閉。 |
| **102** | NTDS ISAM | NTDS (536) NTDSA： database engine (6.02.8225.0000) 正在啟動新的實例 (0) 。 |
| **326** | NTDS ISAM | NTDS (536) NTDSA： database engine 附加資料庫 (1，C:\Windows\NTDS\ntds.dit) 。 (時間=0 秒)<p>內部計時順序：[1] 0.000、[2] 0.015、[3] 0.016、[4] 0.000、[5] 0.031、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.000、[12] 0.000。<p>已儲存快取：1 |
| **105** | NTDS ISAM | NTDS (536) NTDSA：資料庫引擎已啟動新的實例 (0) 。 (時間=1 秒)<p>內部計時順序：[1] 0.031、[2] 0.000、[3] 0.000、[4] 0.391、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.031、[10] 0.000、[11] 0.000。 |
| **1109** | ActiveDirectory_DomainService | 這個目錄伺服器的 invocationID 屬性已變更。 在建立備份時最高的更新序號如下：<p>InvocationID 屬性 (舊值)：<p>*<GUID>*<p>InvocationID 屬性 (新值)：<p>*<GUID>*<p>更新序號：<p>*<Number>*<p>當目錄伺服器從備份媒體還原、設定為裝載可寫入的應用程式目錄分割，或在套用虛擬機器快照後、虛擬機器匯入作業後或即時移轉作業後恢復執行時，invocationID 即會變更。 虛擬的網域控制站不應使用虛擬機器快照來還原。 要還原或復原 Active Directory 網域服務資料庫的內容，支援的方法是還原以 Active Directory 網域服務感知備份應用程式建立的系統狀態備份。 |
| **1168** | ActiveDirectory_DomainService | 內部錯誤：發生 Active Directory Domain Services 錯誤。<p>其他資料<p>錯誤值 (十進位)：<p>2<p>錯誤值 (十六進位)：<p>2<p>內部識別碼：<p>7011658 |
| **1110** | ActiveDirectory_DomainService | 將這個網域控制站升級為通用類別目錄將會延遲下列間隔。<p>間隔 (分鐘)：<p>5<p>這個延遲是必要的，這樣在通告通用類別目錄前，可以先準備所需的目錄分割。 在登錄中，您可以指定將本機網域控制站升級為通用類別目錄前，目錄系統代理程式將等候的秒數。 請參閱《Resource Kit Distributed Systems Guide》，以取得有關通用類別目錄延遲通告登錄值的資訊 |
| **103** | NTDS ISAM | NTDS (536) NTDSA：資料庫引擎已停止實例 (0) 。<p>中途關機：0<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.047、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.016、[10] 0.000、[11] 0.000、[12] 0.000、[13] 0.000、[14] 0.000、[15] 0.000。 |
| **1004** | ActiveDirectory_DomainService | Active Directory 網域服務成功地關閉。 |
| **1539** | ActiveDirectory_DomainService | Active Directory 網域服務無法停用下列硬碟上的軟體磁碟寫入快取。<p>硬碟：<p>c:<p>資料可能已在系統失敗時遺失 |
| **2179** | ActiveDirectory_DomainService | 網域控制站電腦物件的 msDS-GenerationId 屬性已設為下列參數：<p>GenerationID 屬性：<p>*<Number>* |
| **2173** | ActiveDirectory_DomainService | 無法讀取網域控制站電腦物件的 msDS-GenerationId 屬性。 造成此問題的原因，可能是資料庫交易失敗或世代識別碼不存在本機資料庫。 msDS-GenerationId 在 dcpromo 後的第一次重新開機期間不存在，或 DC 不是虛擬網域控制站。<p>其他資料<p>失敗碼：<p>6 |
| **1000** | ActiveDirectory_DomainService | Microsoft Active Directory 網域服務啟動完成，版本 6.2.8225.0 |
| **1394** | ActiveDirectory_DomainService | 已經清除所有妨礙更新至 Active Directory 網域服務資料庫的問題。 已經成功更新 Active Directory 網域服務資料庫。 Net Logon 服務已經重新啟動。 |
| **1128** | ActiveDirectory_DomainService | 1128 知識一致性檢查程式「已建立一個從下列來源目錄服務到本機目錄服務的複寫連線。<p>來源目錄服務：<p>CN = NTDS 設定， *<Domain Controller DN>*<p>本機目錄服務：<p>CN = NTDS 設定， *<Domain Controller DN>*<p>其他資料<p>原因代碼：<p>0x2<p>建立點內部識別碼：<p>f0a025d |
| **1999** | ActiveDirectory_DomainService | 來源目錄服務已最佳化由目的地目錄服務所提供的更新序號 (USN)。 來源及目的地目錄服務有公用的複寫協力電腦。 目的地目錄服務和公用的複寫協力電腦包含一樣的最新資料，而來源目錄服務是使用這台協力電腦的備份來安裝。<p>目的地目錄服務識別碼：<p>*<GUID> (<FQDN>)*<p>公用目錄服務識別碼：<p>*<GUID>*<p>公用屬性 USN：<p>*<Number>*<p>因此，已使用下列設定值來設定目的地目錄服務的即時性向量。<p>先前的物件 USN：<p>0<p>先前的屬性 USN：<p>0<p>資料庫 GUID：<p>*<GUID>*<p>物件 USN：<p>*<Number>*<p>屬性 USN：<p>*<Number>* |

##### <a name="system-event-log"></a>系統事件記錄檔
複製作業之後的指示位於 [系統] 事件記錄檔中。 當 Hypervisor 告知客體電腦它是從快照複製或還原時，網域控制站會立即使它的 RID 集區失效，以避免稍後複製安全性主體。 當複製繼續進行時，各種預期的作業與訊息即會出現，大多數會與服務啟動和停止有關，而這會導致一些預期的錯誤發生。 完成時，[系統] 事件記錄檔會記錄整體複製成功。

|**事件識別碼**|**Source**|**Message**|
|--|--|--|
|**16654**|Directory-Services-SAM|帳戶識別碼 (RID) 集區成為無效。 在下列預期情況中可能會發生此問題：<p>1. 從備份還原網域控制站。<p>2. 在虛擬機器上執行的網域控制站會從快照集還原。<p>3. 系統管理員已手動將集區失效|
|**7036**|服務控制管理員|Active Directory 網域服務已進入執行狀態。|
|**7036**|服務控制管理員|Kerberos 金鑰發佈中心服務已進入執行狀態。|
|**3096**|Netlogon|找不到這個網域的網域主控站。|
|**7036**|服務控制管理員|安全性帳戶管理員服務已進入執行狀態。|
|**7036**|服務控制管理員|伺服器服務已進入執行狀態。|
|**7036**|服務控制管理員|Netlogon 服務已進入執行狀態。|
|**7036**|服務控制管理員|Active Directory Web 服務已進入執行狀態。|
|**7036**|服務控制管理員|DFS 複寫服務已進入執行狀態。|
|**7036**|服務控制管理員|檔案複寫服務已進入執行狀態。|
|**14533**|Microsoft-Windows-DfsSvc|DFS 已經完成建立所有命名空間。|
|**14531**|Microsoft-Windows-DfsSvc|DFS 伺服器完成初始化。|
|**7036**|服務控制管理員|DFS 命名空間服務已進入執行狀態。|
|**7023**|服務控制管理員|站台間訊息服務因下列錯誤而終止：<p>指定的伺服器無法執行要求的作業。|
|**7036**|服務控制管理員|站台間訊息服務已進入停止狀態。|
|**5806**|Netlogon|已手動停用此網域控制站上的動態更新。<p>使用者動作<p>重新設定此網域控制站以使用動態更新，或者手動將檔案 '%SystemRoot%\System32\Config\Netlogon.dns' 的 DNS 記錄新增到 DNS 資料庫。」|
|**16651**|Directory-Services-SAM|要求新帳戶-識別元集區失敗。 將會重試作業，直到要求成功為止。 錯誤碼為<p>要求的 FSMO 作業失敗。 無法連絡目前的 FSMO 持有人。|
|**7036**|服務控制管理員|DFS 伺服器服務已進入執行狀態。|
|**7036**|服務控制管理員|DFS 角色伺服器服務已進入執行狀態。|
|**7036**|服務控制管理員|Netlogon 服務已進入停止狀態。|
|**7036**|服務控制管理員|檔案複寫服務已進入停止狀態。|
|**7036**|服務控制管理員|Kerberos 金鑰發佈中心服務已進入停止狀態。|
|**7036**|服務控制管理員|DFS 伺服器服務已進入停止狀態。|
|**7036**|服務控制管理員|Active Directory 網域服務已進入停止狀態。|
|**7036**|服務控制管理員|Netlogon 服務已進入執行狀態。|
|**7040**|服務控制管理員|Active Directory 網域服務的啟動類型已從自動啟動變更為停用。|
|**7036**|服務控制管理員|Netlogon 服務已進入停止狀態。|
|**7036**|服務控制管理員|檔案複寫服務已進入執行狀態。|
|**29219**|DirectoryServices-DSROLE-Server|虛擬網域控制站複製成功。|
|**29223**|DirectoryServices-DSROLE-Server|這個伺服器現在已經是網域控制站。|
|**29265**|DirectoryServices-DSROLE-Server|虛擬網域控制站複製成功。 虛擬網域控制站複製設定檔 C:\Windows\NTDS\DCCloneConfig.xml 已重新命名為 C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml。|
|**1074**|User32|處理 C:\Windows\system32\lsass.exe (DC2) 因為下列原因而已起始電腦 DC2 的重新開機 AUTHORITY\SYSTEM：作業系統：重新設定 (計畫的) <p>原因代碼：0x80020004<p>關機類型：重新啟動<p>註解：「|

##### <a name="dcpromolog"></a>DCPROMO.LOG
Dcpromo.log 包含複製中 [目錄服務] 事件記錄檔不會描述的實際升級部分。 由於記錄檔不提供事件記錄檔項目所傳達的說明層級，因此本單元的這一節將包含額外的註釋。

升級處理程序表示複製啟動、DC 已刪除其目前設定並使用現有的 AD 資料庫重新升級 (非常類似 IFM 升級)，接著 DC 會複寫 AD 和 SYSVOL 的輸入變更差異，然後完成複製。

> [!NOTE]
> 本單元中的記錄檔已經過修改，移除了資料欄位來提高可讀性。
>
> 如需 dcpromo.log 的進一步說明，請參閱＜了解及疑難排解 Windows Server 2012　中的 AD DS 簡化管理＞。
>
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)

- 開始以複製為基礎的升級

- 設定 [目錄服務還原模式] 旗標，讓伺服器不會和原始複製品一樣正常進行開機備份，因而導致命名或 [目錄服務] 衝突。

- 更新 [目錄服務] 事件記錄檔

```
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.
```

- 停止 NetLogon 服務，讓網域控制站不會進行通告

```
15:14:01 [INFO] Stopping service NETLOGON
15:14:01 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)
15:14:01 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states
15:14:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3
15:14:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1
15:14:02 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state
15:14:02 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0
15:14:02 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)
15:14:02 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state
15:14:02 [INFO] StopService on NETLOGON returned 0
15:14:02 [INFO] Configuring service NETLOGON to 1 returned 0
15:14:02 [INFO] Updating service status to 4
15:14:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- 檢查 dccloneconfig.xml 檔案中系統管理員特定的自訂項目。

- 在這個範例中，它是一個空白檔案，因此，所有設定都會自動產生，且需要從網路進行自動 IP 定址

```
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0
```

- 驗證未安裝任何不屬於 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 的服務或程式

```
15:14:02 [INFO] vDC Cloning: Checking allowed list:
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- 啟用網路介面卡上的 DHCP，因為系統管理員並未指定 IP 資訊

```
15:14:03 [INFO] vDC Cloning: Enable DHCP:
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12
15:14:03 [INFO] Method: EnableDHCP
15:14:03 [INFO] HRESULT code: 0x0 (0)
15:14:03 [INFO] Return Value: 0x0 (0)
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- 尋找 PDC 模擬器

- 設定複製品的站台 (在此案例中是自動產生的)

- 設定複製品的名稱 (在此案例中是自動產生的)

```
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name
```

- 建立新的複製電腦物件

- 將複製品重新命名以符合新名稱

```
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)
```

- 根據先前的 dccloneconfig.xml 或自動產生規則來提供升級設定

```
15:14:05 [INFO] vDC Cloning: Promotion parameters setting:
15:14:05 [INFO] DNS Domain Name: root.fabrikam.com
15:14:05 [INFO] Replica Partner: \\DC1.root.fabrikam.com
15:14:05 [INFO] Site Name: Default-First-Site-Name
15:14:05 [INFO] DS Database Path: C:\Windows\NTDS
15:14:05 [INFO] DS Log Path: C:\Windows\NTDS
15:14:05 [INFO] SysVol Root Path: C:\Windows\SYSVOL
15:14:05 [INFO] Account: root.fabrikam.com\DC2-CL0001$
15:14:05 [INFO] Options: DSROLE_DC_CLONING (0x800400)
```

- 開始升級

```
15:14:05 [INFO] Promote DC as a clone
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #3: Domain Controller cloning is at 15% completion...
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #4: Domain Controller cloning is at 16% completion...
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:05 [INFO] Validate supplied paths
15:14:05 [INFO] Validating path C:\Windows\NTDS.
15:14:05 [INFO] Path is a directory
15:14:05 [INFO] Path is on a fixed disk drive.
15:14:05 [INFO] Validating path C:\Windows\NTDS.
15:14:05 [INFO] Path is a directory
15:14:05 [INFO] Path is on a fixed disk drive.
15:14:05 [INFO] Validating path C:\Windows\SYSVOL.
15:14:05 [INFO] Path is on a fixed disk drive.
15:14:05 [INFO] Path is on an NTFS volume
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #5: Domain Controller cloning is at 17% completion...
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:05 [INFO] Start the worker task
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #6: Domain Controller cloning is at 20% completion...
15:14:05 [INFO] Request for promotion returning 0
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #7: Domain Controller cloning is at 21% completion...
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- 停止並設定所有與 AD DS 相關的服務 (NTDS、NTFRS/DFSR、KDC、DNS)

> [!NOTE]
> 在這個案例中預期 DNS 服務需要長時間進行關機，因為它使用的是無法再使用的 AD 整合區域 (即使是在停止 NTDS 服務之前也一樣)，請參閱本單元這一節稍後描述的 DNS 事件。

```
15:14:15 [INFO] Stopping service NTDS
15:14:15 [INFO] Stopping service NtFrs
15:14:15 [INFO] ControlService(STOP) on NtFrs returned 1(gle=0)
15:14:15 [INFO] DsRolepWaitForService: waiting for NtFrs to enter one of 7 states
15:14:15 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=3
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on NtFrs returned 1 (gle=0), SvcStatus.dwCS=1
15:14:16 [INFO] DsRolepWaitForService: exiting because NtFrs entered STOPPED state
15:14:16 [INFO] DsRolepWaitForService(for any end state) on NtFrs service returned 0
15:14:16 [INFO] ControlService(STOP) on NtFrs returned 0(gle=1062)
15:14:16 [INFO] Exiting service-stop loop after service NtFrs entered STOPPED state
15:14:16 [INFO] StopService on NtFrs returned 0
15:14:16 [INFO] Configuring service NtFrs to 1 returned 0
15:14:16 [INFO] Stopping service Kdc
15:14:16 [INFO] ControlService(STOP) on Kdc returned 1(gle=0)
15:14:16 [INFO] DsRolepWaitForService: waiting for Kdc to enter one of 7 states
15:14:16 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=3
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on Kdc returned 1 (gle=0), SvcStatus.dwCS=1
15:14:17 [INFO] DsRolepWaitForService: exiting because Kdc entered STOPPED state
15:14:17 [INFO] DsRolepWaitForService(for any end state) on Kdc service returned 0
15:14:17 [INFO] ControlService(STOP) on Kdc returned 0(gle=1062)
15:14:17 [INFO] Exiting service-stop loop after service Kdc entered STOPPED state
15:14:17 [INFO] StopService on Kdc returned 0
15:14:17 [INFO] Configuring service Kdc to 1 returned 0
15:14:17 [INFO] Stopping service DNS
15:14:17 [INFO] ControlService(STOP) on DNS returned 1(gle=0)
15:14:17 [INFO] DsRolepWaitForService: waiting for DNS to enter one of 7 states
15:14:17 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:18 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:19 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:20 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:21 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:22 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:23 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:24 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:25 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:26 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:27 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:28 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:29 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:30 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:31 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:32 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:33 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:34 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:35 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:36 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:37 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:38 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:39 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:40 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:41 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:42 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:43 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:44 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:45 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:46 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:47 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:48 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:49 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:50 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:51 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:52 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:53 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:54 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:55 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:56 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:57 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:58 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:14:59 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=3
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on DNS returned 1 (gle=0), SvcStatus.dwCS=1
15:15:00 [INFO] DsRolepWaitForService: exiting because DNS entered STOPPED state
15:15:00 [INFO] DsRolepWaitForService(for any end state) on DNS service returned 0
15:15:00 [INFO] ControlService(STOP) on DNS returned 0(gle=1062)
15:15:00 [INFO] Exiting service-stop loop after service DNS entered STOPPED state
15:15:00 [INFO] StopService on DNS returned 0
15:15:00 [INFO] Configuring service DNS to 1 returned 0
15:15:00 [INFO] ControlService(STOP) on NTDS returned 1(gle=1062)
15:15:00 [INFO] DsRolepWaitForService: waiting for NTDS to enter one of 7 states
15:15:00 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=3
15:15:01 [INFO] DsRolepWaitForService: QueryServiceStatus on NTDS returned 1 (gle=0), SvcStatus.dwCS=1
15:15:01 [INFO] DsRolepWaitForService: exiting because NTDS entered STOPPED state
15:15:01 [INFO] DsRolepWaitForService(for any end state) on NTDS service returned 0
15:15:01 [INFO] ControlService(STOP) on NTDS returned 0(gle=1062)
15:15:01 [INFO] Exiting service-stop loop after service NTDS entered STOPPED state
15:15:01 [INFO] StopService on NTDS returned 0
15:15:01 [INFO] Configuring service NTDS to 1 returned 0
15:15:01 [INFO] Configuring service NTDS
15:15:01 [INFO] Configuring service NTDS to 64 returned 0
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #8: Domain Controller cloning is at 22% completion...
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:01 [INFO] vDC Cloning: Winlogon UI Notification #9: Domain Controller cloning is at 25% completion...
15:15:01 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- 強制執行與另一個網域控制站 (通常是 PDCE) 的 NT5DS (NTP) 時間同步處理

```
15:15:02 [INFO] Forcing time sync
```

- 連絡持有複製品之來源網域控制站帳戶的網域控制站

- 排清任何現有的 Kerberos 票證

```
15:15:02 [INFO] Searching for a domain controller for the domain root.fabrikam.com that contains the account DC2$
15:15:02 [INFO] Located domain controller DC1.root.fabrikam.com for domain root.fabrikam.com
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #10: Domain Controller cloning is at 26% completion...
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:02 [INFO] Directing kerberos authentication to DC1.root.fabrikam.com returns 0
15:15:02 [INFO] DsRolepFlushKerberosTicketCache() successfully flushed the Kerberos ticket cache
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #11: Domain Controller cloning is at 27% completion...
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:02 [INFO] Using site Default-First-Site-Name for server \\DC1.root.fabrikam.com
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:02 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- 停止 NetLogon 服務並設定它的啟動類型

```
15:15:02 [INFO] Stopping service NETLOGON
15:15:02 [INFO] Stopping service NETLOGON
15:15:02 [INFO] vDC Cloning: Winlogon UI Notification #12: Domain Controller cloning is at 29% completion...
15:15:02 [INFO] ControlService(STOP) on NETLOGON returned 1(gle=0)
15:15:02 [INFO] DsRolepWaitForService: waiting for NETLOGON to enter one of 7 states
15:15:02 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=3
15:15:03 [INFO] DsRolepWaitForService: QueryServiceStatus on NETLOGON returned 1 (gle=0), SvcStatus.dwCS=1
15:15:03 [INFO] DsRolepWaitForService: exiting because NETLOGON entered STOPPED state
15:15:03 [INFO] DsRolepWaitForService(for any end state) on NETLOGON service returned 0
15:15:03 [INFO] ControlService(STOP) on NETLOGON returned 0(gle=1062)
15:15:03 [INFO] Exiting service-stop loop after service NETLOGON entered STOPPED state
15:15:03 [INFO] StopService on NETLOGON returned 0
15:15:03 [INFO] Configuring service NETLOGON to 1 returned 0
15:15:03 [INFO] Stopped NETLOGON
15:15:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:03 [INFO] vDC Cloning: Winlogon UI Notification #13: Domain Controller cloning is at 30% completion...
```

- 將 DFSR/NTFRS 服務設定為自動執行

- 刪除其現有的資料庫檔案，以便在服務下一次啟動時，強制執行 SYSVOL 的非權威同步處理

```
15:15:03 [INFO] Configuring service DFSR
15:15:03 [INFO] Configuring service DFSR to 256 returned 0
15:15:03 [INFO] Configuring service NTFRS
15:15:03 [INFO] Configuring service NTFRS to 256 returned 0
15:15:03 [INFO] Removing DFSR Database files for SysVol
15:15:03 [INFO] Removing FRS Database files in C:\Windows\ntfrs\jet
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edb.log
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00001.jrs
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbres00002.jrs
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\log\edbtmp.log
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\ntfrs.jdb
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\sys\edb.chk
15:15:03 [INFO] Removed C:\Windows\ntfrs\jet\temp\tmp.edb
15:15:04 [INFO] Created system volume path
15:15:04 [INFO] Configuring service DFSR
15:15:04 [INFO] Configuring service DFSR to 128 returned 0
15:15:04 [INFO] Configuring service NTFRS
15:15:04 [INFO] Configuring service NTFRS to 128 returned 0
15:15:04 [INFO] vDC Cloning: Winlogon UI Notification #14: Domain Controller cloning is at 40% completion...
15:15:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.
```

- 使用現有的 NTDS 資料庫檔案開始升級處理程序

- 連絡 RID 主機

> [!NOTE]
> AD DS 服務並未實際安裝於此處，這是記錄檔中舊有的檢測

```
15:15:04 [INFO] Installing the Directory Service
15:15:04 [INFO] Calling NtdsInstall for root.fabrikam.com
15:15:04 [INFO] Starting Active Directory Domain Services installation
15:15:04 [INFO] Validating user supplied options
15:15:04 [INFO] Determining a site in which to install
15:15:04 [INFO] Examining an existing forest...
15:15:04 [INFO] Starting a replication cycle between DC1.root.fabrikam.com and the RID operations master (2008r2-01.root.fabrikam.com), so that the new replica will be able to create users, groups, and computer objects...
15:15:04 [INFO] Configuring the local computer to host Active Directory Domain Services
15:15:04 [INFO] EVENTLOG (Warning): NTDS General / Service Control : 1539
Active Directory Domain Services could not disable the software-based disk write cache on the following hard disk.
Hard disk:
c:
Data might be lost during system failures.
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Processing : 2041
Duplicate event log entries were suppressed.
See the previous event log entry for details. An entry is considered a duplicate if
the event code and all of its insertion parameters are identical. The time period for
this run of duplicates is from the time of the previous event to the time of this event.
Event Code:
80000603
Number of duplicate entries:
2
15:15:10 [INFO] EVENTLOG (Informational): NTDS General / Internal Configuration : 2121
This Active Directory Domain Services server is disabling the Recycle Bin. Deleted objects may not be undeleted at this time.
```

- 變更存在於來源電腦資料庫中現有的呼叫識別碼

- 針對這個複製品建立新的 NTDS 設定物件

- 在夥伴網域控制站的 AD 物件差異中進行複寫

> [!NOTE]
> 即使所有物件皆列示為已複寫，這也只是納入更新所需的中繼資料而已。 複製的 NTDS 資料庫中所有未變更的物件都已經存在，而且不需要再次複寫，就像是使用以 IFM 為基礎的升級一樣。

```
15:15:10 [INFO] EVENTLOG (Informational): NTDS Replication / Replication : 1109
The invocationID attribute for this directory server has been changed. The highest update sequence number at the time the backup was created is as follows:
InvocationID attribute (old value):
24e7b22f-4706-402d-9b4f-f2690f730b40
InvocationID attribute (new value):
f74cefb2-89c2-442c-b1ba-3234b0ed62f8
Update sequence number:
20520
The invocationID is changed when a directory server is restored from backup media, is configured to host a writeable application directory partition, has been resumed after a virtual machine snapshot has been applied, after a virtual machine import operation, or after a live migration operation. Virtualized domain controllers should not be restored using virtual machine snapshots. The supported method to restore or rollback the content of an Active Directory Domain Services database is to restore a system state backup made with an Active Directory Domain Services-aware backup application.
15:15:10 [INFO] EVENTLOG (Error): NTDS General / Internal Processing : 1168
Internal error: An Active Directory Domain Services error has occurred.
Additional Data
Error value (decimal):
2
Error value (hexadecimal):
2
Internal ID:
7011658
15:15:11 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC1.root.fabrikam.com...
15:15:11 [INFO] Replicating the schema directory partition
15:15:11 [INFO] Replicated the schema container.
15:15:12 [INFO] Active Directory Domain Services updated the schema cache.
15:15:12 [INFO] Replicating the configuration directory partition
15:15:12 [INFO] Replicating data CN=Configuration,DC=root,DC=fabrikam,DC=com: Received 2612 out of approximately 2612 objects and 94 out of approximately 94 distinguished name (DN) values...
15:15:12 [INFO] Replicated the configuration container.
15:15:13 [INFO] Replicating critical domain information...
15:15:13 [INFO] Replicating data DC=root,DC=fabrikam,DC=com: Received 109 out of approximately 109 objects and 35 out of approximately 35 distinguished name (DN) values...
15:15:13 [INFO] Replicated the critical objects in the domain container.
```

- 視需要使用任何遺失的更新來填入 GC 磁碟分割

- 完成升級的重要 AD DS 部分

```
15:15:13 [INFO] EVENTLOG (Informational): NTDS General / Global Catalog : 1110
Promotion of this domain controller to a global catalog will be delayed for the following interval.
Interval (minutes):
5
This delay is necessary so that the required directory partitions can be prepared before the global catalog is advertised. In the registry, you can specify the number of seconds that the directory system agent will wait before promoting the local domain controller to a global catalog. For more information about the Global Catalog Delay Advertisement registry value, see the Resource Kit Distributed Systems Guide.
15:15:14 [INFO] EVENTLOG (Informational): NTDS General / Service Control : 1000
Microsoft Active Directory Domain Services startup complete, version 6.2.8225.0
15:15:15 [INFO] Creating new domain users, groups, and computer objects
15:15:16 [INFO] Completing Active Directory Domain Services installation
15:15:16 [INFO] NtdsInstall for root.fabrikam.com returned 0
15:15:16 [INFO] DsRolepInstallDs returned 0
15:15:16 [INFO] Installed Directory Service
```

- 完成 SYSVOL 的輸入複寫

```
15:15:16 [INFO] vDC Cloning: Winlogon UI Notification #15: Domain Controller cloning is at 60% completion...
15:15:16 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] Completed system volume replication
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #16: Domain Controller cloning is at 70% completion...
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] SetProductType to 2 [LanmanNT] returned 0
15:15:18 [INFO] Set the product type
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #17: Domain Controller cloning is at 71% completion...
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #18: Domain Controller cloning is at 72% completion...
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] Set the system volume path for NETLOGON
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #19: Domain Controller cloning is at 73% completion...
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] Replicating non critical information
15:15:18 [INFO] User specified to not replicate non-critical data
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #20: Domain Controller cloning is at 80% completion...
15:15:18 [INFO] Stopped the DS
15:15:18 [INFO] vDC Cloning: Set vDCCloningUpdate event.
15:15:18 [INFO] vDC Cloning: Winlogon UI Notification #21: Domain Controller cloning is at 90% completion...
15:15:18 [INFO] Configuring service NTDS
15:15:18 [INFO] Configuring service NTDS to 16 returned 0
```

- 啟用用戶端 DNS 登錄

```
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.
```

- 執行 DefaultDCCloneAllowList.xml <SysprepInformation> 元素所指定的 SYSPREP 模組。

```
15:15:18 [INFO] vDC Cloning: Running sysprep providers.
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.
```

- 完成複製升級

- 移除 DSRM 開機旗標，讓伺服器下一次能夠正常開機

- 重新命名 dccloneconfig.xml，以便在下一次開機時不會再次讀取該檔案

- 重新啟動電腦

```
15:15:32 [INFO] The attempted domain controller operation has completed
15:15:32 [INFO] Updating service status to 4
15:15:32 [INFO] DsRolepSetOperationDone returned 0
15:15:32 [INFO] vDC Cloning: Set vDCCloningComplete event.
15:15:32 [INFO] vDC Cloneing: Clearing Boot into DSRM flag succeeded.
15:15:32 [INFO] vDC Cloning: Winlogon UI Notification #22: Cloning Domain Controller succeeded. Now rebooting...
15:15:33 [INFO] vDC Cloning: Renamed vDC clone configuration file.
15:15:33 [INFO] vDC Cloning: The old name is: C:\Windows\NTDS\DCCloneConfig.xml
15:15:33 [INFO] vDC Cloning: The new name is: C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml
15:15:34 [INFO] vDC Cloning: Release Ipv4 on interface 'Wired Ethernet Connection 2', result=0.
15:15:34 [INFO] vDC Cloning: Release Ipv6 on interface 'Wired Ethernet Connection 2', result=0.
15:15:34 [INFO] Rebooting machine
```

##### <a name="active-directory-web-services-event-log"></a>Active Directory Web 服務事件記錄檔
發生複製時，NTDS.DIT 資料庫通常會離線很長一段時間。 ADWS 服務至少會針對此情況記錄一個事件。 複製完成後，ADWS 服務會啟動、記錄尚未有有效的電腦憑證 (根據您部署 Microsoft PKI (不一定會自動註冊) 的環境而定，不一定會有憑證)，然後啟動新網域控制站的執行個體。

|**事件識別碼**|**Source**|**Message**|
|--|--|--|
|**1202**|ADWS 執行個體事件|這個電腦目前裝載指定的目錄執行個體，但是 Active Directory Web 服務無法對它提供服務。 Active Directory Web 服務會定期重試這個作業。<p>目錄實例： NTDS<p>目錄實例 LDAP 埠：389<p>目錄實例 SSL 埠：636|
|**1000**|ADWS 執行個體事件|正在啟動 Active Directory Web 服務|
|**1008**|ADWS 執行個體事件|Active Directory Web 服務已順利降低它的安全性權限|
|**1100**|ADWS 執行個體事件|已載入在 Active Directory Web 服務之設定檔的 <appsettings> 區段中指定的值，而且未發生錯誤。|
|**1400**|ADWS 執行個體事件|ADWS 憑證事件「Active Directory Web 服務找不到指定憑證名稱的伺服器憑證。 使用 SSL/TLS 連線時必須有憑證。 若要使用 SSL/TLS 連線，請確認電腦上已安裝來自信任的憑證授權單位 (CA) 的有效伺服器驗證憑證。<p>憑證名稱： *<Server FQDN>*|
|**1100**|ADWS 執行個體事件|已載入在 Active Directory Web 服務之設定檔的 <appsettings> 區段中指定的值，而且未發生錯誤。|
|**1200**|ADWS 執行個體事件|Active Directory Web 服務現在正在服務指定的目錄執行個體。<p>目錄實例： NTDS<p>目錄實例 LDAP 埠：389<p>目錄實例 SSL 埠：636|

##### <a name="dns-server-event-log"></a>DNS 伺服器事件記錄檔
DNS 服務會在複製發生時短暫預期的中斷，因為 DNS 服務在 AD DS 資料庫離線時仍會執行。 如果使用 Active Directory 整合的 DNS 便會發生此問題，但若使用標準主要或次要 DNS 則否。 這些錯誤會多次記錄。 複製完成後，DNS 即會恢復正常上線狀態。

|**事件識別碼**|**Source**|**Message**|
|--|--|--|
|**4013**|DNS 伺服器服務|DNS 伺服器正在等待 Active Directory 網域服務 (AD DS) 發出目錄初始同步處理完成的信號。 因為重要 DNS 資料可能尚未複寫到這個網域控制站，所以除非完成初始同步處理，否則無法啟動 DNS 伺服器服務。 如果 AD DS 事件記錄檔中的事件指出發生 DNS 名稱解析問題，則請考慮將這個網域之另一個 DNS 伺服器的 IP 位址新增到這部電腦之網際網路通訊協定內容的 DNS 伺服器清單中。 除非 AD DS 已發出初始同步處理順利完成的信號，否則這個事件會每兩分鐘記錄一次。|
|**4015**|DNS 伺服器服務|DNS 伺服器發生 Active Directory 的嚴重錯誤。 請檢查 Active Directory 是否正常運作。 延伸的錯誤偵錯資訊是 (可能沒有任何資訊) """"。 事件資料包含錯誤。|
|**4000**|DNS 伺服器服務|DNS 伺服器無法開啟 Active Directory。 這台 DNS 伺服器被設定成要取得並使用此區域的目錄資訊，如果無法取得目錄資訊便無法載入區域。 請檢查 Active Directory 是否運作正常，並重新載入區域。 事件資料即錯誤碼。|
|**4013**|DNS 伺服器服務|DNS 伺服器正在等待 Active Directory 網域服務 (AD DS) 發出目錄初始同步處理完成的信號。 因為重要 DNS 資料可能尚未複寫到這個網域控制站，所以除非完成初始同步處理，否則無法啟動 DNS 伺服器服務。 如果 AD DS 事件記錄檔中的事件指出發生 DNS 名稱解析問題，則請考慮將這個網域之另一個 DNS 伺服器的 IP 位址新增到這部電腦之網際網路通訊協定內容的 DNS 伺服器清單中。 除非 AD DS 已發出初始同步處理順利完成的信號，否則這個事件會每兩分鐘記錄一次。|
|**2**|DNS 伺服器服務|DNS 伺服器已經啟動。|
|**4**|DNS 伺服器服務|DNS 伺服器已完成在背景載入區域。 現在所有區域 (只要個別區域設定允許) 都可進行 DNS 更新及區域轉送。|

##### <a name="file-replication-service-event-log"></a>檔案複寫服務事件記錄檔
檔案複寫服務會在複製期間，進行來自合作夥伴的非授權同步處理。 複製完成這個動作的方式是透過刪除 NTFRS 資料庫檔案並讓 SYSVOL 的內容保留不變，以用來做為預先植入的資料。 預期會進行兩個同步處理的嘗試動作。

| **事件識別碼** | **Source** | **Message** |
|--|--|--|
| **13562** | NtFrs | 以下是當輪詢網域控制站 DC2.root.fabrikam.com 的 FRS 複本集設定資訊時，檔案複寫服務所遇到的警告及錯誤摘要。<p>無法連結到網域控制站。 將在下次輪詢循環時重試 |
| **13502** | NtFrs | 檔案複寫服務正在停止。 |
| **13565** | NtFrs | 檔案複寫服務正在使用其他網域控制站的資料來初始化系統磁碟區。 除非這個處理程序完成，否則電腦 DC2 無法變成網域控制站。 系統磁碟區接著會被視為 SYSVOL 加以共用。<p>如果要檢查 SYSVOL 共用，請在命令提示字元下輸入：<p>net share<p>當檔案複寫服務完成初始化程序，SYSVOL 共用將會出現。<p>系統磁碟區初始化會花費一些時間。 花費的時間視系統磁碟區資料量、其他網域控制站可用性，以及網域控制站之間的複寫間隔而定。 |
| **13501** | NtFrs | 檔案複寫服務正在啟動 |
| **13502** | NtFrs | 檔案複寫服務正在停止。 |
| **13503** | NtFrs | 檔案複寫服務已停止。 |
| **13565** | NtFrs | 檔案複寫服務正在使用其他網域控制站的資料來初始化系統磁碟區。 除非這個處理程序完成，否則電腦 DC2 無法變成網域控制站。 系統磁碟區接著會被視為 SYSVOL 加以共用。<p>如果要檢查 SYSVOL 共用，請在命令提示字元下輸入：<p>net share<p>當檔案複寫服務完成初始化程序，SYSVOL 共用將會出現。<p>系統磁碟區初始化會花費一些時間。 花費的時間視系統磁碟區資料量、其他網域控制站可用性，以及網域控制站之間的複寫間隔而定。 |
| **13501** | NtFrs | 檔案複寫服務正在啟動。 |
| **13553** | NtFrs | 檔案複寫服務成功地將這台電腦加入下列複本集：<p>"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<p>這個事件的相關資訊顯示在下方：<p>電腦 DNS 名稱是  *<Domain Controller FQDN>*<p>複本集成員名稱是 *<Domain Controller>*<p>複本集根路徑是 *<path>*<p>複本臨時目錄路徑為 *<path>*<p>複本工作目錄路徑是 *<path>* |
| **13520** | NtFrs | 檔案複寫服務將預先存在的檔案移 <path> 至 *<path>* \ NtFrs_PreExisting___See_EventLog。<p>檔案複寫服務可能會隨時刪除 \ NtFrs_PreExisting___See_EventLog 中的檔案 *<path>* 。 您可以將檔案複製到 \ NtFrs_PreExisting___See_EventLog，以從刪除中儲存檔案 *<path>* 。 如果檔案已經存在其他的複寫協力電腦上，將檔案複製到 c:\windows\sysvol\domain 可能導致名稱衝突。<p>在某些情況下，檔案複寫服務可能會將檔案從 *<path>* \ NtFrs_PreExisting___See_EventLog 複製到， *<path>* 而不是從其他複寫的夥伴複製檔案。<p>您可以藉由刪除 \ NtFrs_PreExisting___See_EventLog 中的檔案，隨時復原空間 *<path>* 。 |
| **13508** | NtFrs | 他的檔案複寫服務無法啟用從到的複寫，以 *\\\\<Domain Controller FQDN>* *<Domain Controller>* *<path>* 使用<p>DNS 名稱 *\\\\<Domain Controller FQDN>* 。 FRS 將繼續重試。<p>出現此警告可能是由下列原因導致。<p>[1] FRS 無法正確解析這部電腦的 DNS 名稱 *\\\\<Domain Controller FQDN>* 。<p>[2] FRS 未在上執行 *\\\\<Domain Controller FQDN>* 。<p>[3] 這個複本在 Active Directory 網域服務上的拓撲資訊尚未複寫到所有網域控制站上。<p>這個事件記錄檔訊息會在每次連線時出現一次。在問題解決之後，您會看到其他的事件記錄檔訊息，指出連線已經建立。 |
| **13509** | NtFrs | 在 *\\\\<Domain Controller FQDN>* *<Domain Controller>* *<Path>* 重複重試之後，檔案複寫服務已啟用從到的複寫。 |
| **13516** | NtFrs | 檔案複寫服務不會再防止電腦 *<Domain Controller>* 成為網域控制站。 系統磁碟區已經成功地初始化，而且已通知 Netlogon 服務，系統磁碟區已準備共用為 SYSVOL。<p>請輸入 " net share" 來檢查 SYSVOL 共用。」 |

##### <a name="dfs-replication-event-log"></a>DFS 複寫事件記錄檔
DFSR 服務會在複製期間，進行來自協力電腦的非權威同步處理。 複製完成這個動作的方式是透過刪除 DFSR 資料庫檔案並讓 SYSVOL 的內容保留不變，以用來做為預先植入的資料。 預期會進行兩個同步處理的嘗試動作。

| **事件識別碼** | **Source** | **Message** |
|--|--|--|
| **1004** | DFSR | DFS 複寫服務已啟動。 |
| **1314** | DFSR | DFS 複寫服務已順利設定偵錯記錄檔。<p>其他資訊：<p>Debug Log File Path： C:\Windows\debug |
| **6102** | DFSR | DFS 複寫服務已順利登錄 WMI 提供者 |
| **1206** | DFSR | DFS 複寫服務已順利連絡網域控制站 DC2.corp.contoso.com 以存取設定資訊。 |
| **1210** | DFSR | DFS 複寫服務已順利設定連入複寫要求的 RPC 接聽程式。<p>其他資訊：<p>埠： 0 " |
| **4614** | DFSR | DFS 複寫服務已初始化位於本機路徑 C:\Windows\SYSVOL\domain 的 SYSVOL，而且正在等候執行初始複寫。 複寫的資料夾會維持在初始同步狀態，直到它和協力電腦完成複寫為止。 如果伺服器已經開始升級至網域控制站，網域控制站會在此問題解決之後才通告並開始運作。 若指定的協力電腦也處於初始同步狀態，或此伺服器或同步協力電腦發生共用違規，也會發生此情形。 若此事件是發生於將 SYSVOL 從檔案複寫服務 (FRS) 移轉至 DFS 複寫時，則只有在解決問題之後才會對外複寫變更。 這會導致此伺服器上的 SYSVOL 資料夾與其他網域控制站不同步。<p>其他資訊：<p>複寫的資料夾名稱： SYSVOL 共用<p>複寫資料夾識別碼： *<GUID>*<p>複寫組名：網域系統磁片區<p>複寫群組識別碼： *<GUID>*<p>成員識別碼： *<GUID>*<p>唯讀：0 |
| **4604** | DFSR | DFS 複寫服務已順利初始化位於本機路徑 C:\Windows\SYSVOL\domain 的 SYSVOL 複寫資料夾。 此成員已完成與協力電腦 dc1.corp.contoso.com 的 SYSVOL 初始同步處理作業。 若要檢查 SYSVOL 共用是否存在，請開啟命令提示視窗，然後輸入 "net share"。<p>其他資訊：<p>複寫的資料夾名稱： SYSVOL 共用<p>複寫資料夾識別碼： *<GUID>*<p>複寫組名：網域系統磁片區<p>複寫群組識別碼： *<GUID>*<p>成員識別碼： *<GUID>*<p>同步夥伴： *<domain controller FQDN>* |

## <a name="troubleshooting-virtualized-domain-controller-safe-restore"></a><a name="BKMK_TshootVDCSafeRestore"></a>疑難排解虛擬網域控制站的安全還原功能

### <a name="tools-for-troubleshooting"></a>適用於疑難排解的工具

#### <a name="logging-options"></a>記錄選項
內建的記錄檔是用來疑難排解網域控制站安全快照還原問題的最重要工具。 預設會啟用並設定這所有的記錄檔，以取得最多的詳細資料。

|**運算**|**Log**|
|--|--|
|**建立快照集**|-事件 viewer\Applications 和服務 logs\Microsoft\Windows\Hyper-V-Worker|
|**還原快照**|-事件 viewer\Applications 和服務 logs\Directory 服務<br />-事件檢視器 \windows 記錄 \ 系統<br />-事件檢視器 \windows logs\Application<br />-事件 viewer\Applications 和服務 logs\File Replication Service<br />-事件 viewer\Applications 和服務 logs\DFS 複寫<br />-事件 viewer\Applications 和服務 logs\DNS<br />-事件 viewer\Applications 和服務 logs\Microsoft\Windows\Hyper-V-Worker|

#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>適用於疑難排解網域控制站設定的工具和命令
若要疑難排解記錄檔未說明的問題，請使用下列工具做為起點：

- Dcdiag.exe

- Repadmin.exe

- 網路監視器 3.4

#### <a name="general-methodology-for-troubleshooting-domain-controller-safe-restore"></a><a name="BKMK_TshhotSafeRestore"></a>適用於疑難排解虛擬網域控制站安全還原功能的一般方法

1. 安全快照還原如預期般執行，但發生問題嗎？

    1. 檢查 [目錄服務] 事件記錄檔

        1. 是否發生任何快照還原錯誤？

        2. 是否發生任何 AD 複寫錯誤？

    2. 檢查 [系統] 事件記錄檔

        1. 是否發生任何通訊錯誤？

        2. 是否發生任何 AD 錯誤？

2. 安全快照還原是意外發生的嗎？

    1. 檢查 Hypervisor 稽核記錄，以判斷導致復原的人員或原因

    2. 連絡 Hypervisor 的所有系統管理員，並詢問他們哪些人員在未通知的情況下復原 VM

3. 伺服器正在實作 USN 復原保護，並以不安全的方式進行還原嗎？

    1. 檢查 [目錄服務] 事件記錄檔，以了解不支援的 Hypervisor 或整合服務

    2. 檢查作業系統並驗證執行中的 Windows Server 2012？

### <a name="troubleshooting-specific-problems"></a><a name="BKMK_TshootSpecificSafeRestore"></a>疑難排解特定問題

#### <a name="events"></a>事件
所有的虛擬網域控制站安全快照還原事件都會寫入還原網域控制站 VM 的 [目錄服務] 事件記錄檔。 [應用程式]、[系統]、[檔案複寫服務]，以及 [DFS 複寫] 事件記錄檔可能也會包含關於失敗還原的有用疑難排解資訊。

以下是 [目錄服務] 事件記錄檔中的 Windows Server 2012 安全還原特定的事件。

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2170** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 警告 |
| **Message** | 偵測到世代識別碼變更。<p>在 DS 快取的世代識別碼 (舊值)：%1<p>VM 中目前的世代識別碼 (新值)：%2<p>在使用虛擬機器快照後、虛擬機器匯入作業後或即時移轉作業後會發生世代識別碼變更。 *<COMPUTERNAME>* 將建立新的調用識別碼來復原網域控制站。 虛擬的網域控制站不應使用虛擬機器快照來還原。 要還原或復原 Active Directory 網域服務資料庫的內容，支援的方法是還原以 Active Directory 網域服務感知備份應用程式建立的系統狀態備份。 |
| **注意事項與解決方法** | 如果預期有快照，這是成功的事件。 如果沒有，請檢查 Hyper-V-Worker 事件記錄檔，或者連絡 Hypervisor 系統管理員。 |

| 事件 | 描述 |
| -- |--|
|**事件識別碼**|**2174**|
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|
|**嚴重性**|資訊|
|**Message**|DC 不是虛擬網域控制站複製品，也不是還原的虛擬網域控制站快照。|
|**注意事項與解決方法**|預期在啟動不是從快照還原的實體網域控制站或虛擬網域控制站時所產生的事件|

| 事件 | 描述 |
| -- |--|
|**事件識別碼**|**2181**|
|**Source**|Microsoft-Windows-ActiveDirectory_DomainService|
|**嚴重性**|資訊|
|**Message**|交易已中止，因為虛擬機器正在還原成先前的狀態。 在使用虛擬機器快照後、虛擬機器匯入作業後或即時移轉作業後發生。|
|**注意事項與解決方法**|預期會在還原快照時發生。 交易追蹤 VM 世代識別碼變更|

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2185** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | *<COMPUTERNAME>* 已停止用於複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務。<p>服務名稱：%1<p>Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 *<COMPUTERNAME>* 必須在本機 SYSVOL 複本上初始化非權威還原。 若要這樣做，請停止用來複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務，然後使用適當的登錄機碼與值啟動它以觸發還原。 重新啟動 FRS 或 DFSR 服務時，將記錄事件 2187。 |
| **注意事項與解決方法** | 預期會在還原快照時發生。 此網域控制站上的所有 SYSVOL 資料會都由協力電腦 DC 的複本來取代。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | 2186 |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 無法停止用來複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務。<p>服務名稱：%1<p>錯誤碼：%2<p>錯誤訊息：%3<p>Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 *<COMPUTERNAME>* 必須在本機 SYSVOL 複本上初始化非權威還原。 若要這樣做，請停止用來複寫 SYSVOL 資料夾的 FRS 或 DFSR 複寫服務，然後使用適當的登錄機碼與值啟動它以觸發還原。 *<COMPUTERNAME>* 無法停止目前正在執行的服務，因此無法完成非權威還原。 請手動執行非權威還原。 |
| **注意事項與解決方法** | 檢查 [系統]、[FRS] 及 [DFSR] 事件記錄檔，以取得進一步資訊。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2187** |
| **嚴重性** | 資訊 |
| **Message** | *<COMPUTERNAME>* 已啟動用於複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務。<p>服務名稱：%1<p>Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 *<COMPUTERNAME>* 需要在本機 SYSVOL 複本上初始化非權威還原。 若要這樣做，請停止用來複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務，然後使用適當的登錄機碼與值啟動它以觸發還原。 |
| **注意事項與解決方法** | 預期會在還原快照時發生。 此網域控制站上的所有 SYSVOL 資料會都由協力電腦 DC 的複本來取代。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2188** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 無法啟動用於複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務。<p>服務名稱：%1<p>錯誤碼：%2<p>錯誤訊息：%3<p>Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 *<COMPUTERNAME>* 必須在本機 SYSVOL 複本上初始化非權威還原。 若要這樣做，請停止用來複寫 SYSVOL 的 FRS 或 DFSR 服務，然後使用適當的登錄機碼與值啟動它以觸發還原。 *<COMPUTERNAME>* 無法啟動用於複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務，因此無法完成非權威還原。 請手動執行非權威還原，然後重新啟動服務。 |
| **注意事項與解決方法** | 檢查 [系統]、[FRS] 及 [DFSR] 事件記錄檔，以取得進一步資訊。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2189** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | *<COMPUTERNAME>* 在非權威還原期間，設定下列登錄值以初始化 SYSVOL 複本：<p>登錄機碼：%1<p>登錄值：%2<p>登錄值資料：%3<p>Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 *<COMPUTERNAME>* 必須在本機 SYSVOL 複本上初始化非權威還原。 若要這樣做，請停止用來複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務，然後使用適當的登錄機碼與值啟動它以觸發還原。 |
| **注意事項與解決方法** | 預期會在還原快照時發生。 此網域控制站上的所有 SYSVOL 資料會都由協力電腦 DC 的複本來取代。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2190** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 在非權威還原期間，無法設定下列登錄值以初始化 SYSVOL 複本：<p>登錄機碼：%1<p>登錄值：%2<p>登錄值資料：%3<p>錯誤碼：%4<p>錯誤訊息：%5<p>Active Directory 偵測到主控網域控制站角色的虛擬機器已還原至先前狀態。 *<COMPUTERNAME>* 必須在本機 SYSVOL 複本上初始化非權威還原。 若要這樣做，請停止用來複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務，然後使用適當的登錄機碼與值啟動它以觸發還原。 *<COMPUTERNAME>* 無法設定上述登錄值，因此無法完成非權威還原。 請手動執行非權威還原。 |
| **注意事項與解決方法** | 檢查 [應用程式] 和 [系統] 事件記錄檔。 調查可能封鎖登錄更新的協力廠商應用程式。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2200** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 *<COMPUTERNAME>* 初始化複寫以使網域控制站成為最新的。 當複寫完成時，將記錄事件 2201。 |
| **注意事項與解決方法** | 預期會在還原快照時發生。 標記開始輸入 AD 複寫。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2201** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 *<COMPUTERNAME>* 已完成複寫，使網域控制站成為最新的。 |
| **注意事項與解決方法** | 預期會在還原快照時發生。 標記結束輸入 AD 複寫。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2202** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 *<COMPUTERNAME>* 複寫失敗，使網域控制站維持在最新狀態。 將在下次定期複寫後更新網域控制站。 |
| **注意事項與解決方法** | 檢查 [目錄服務] 和 [系統] 事件記錄檔。 使用 repadmin.exe 嘗試強制執行複寫，並記下任何失敗。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2204** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | *<COMPUTERNAME>* 偵測到虛擬機器世代識別碼有變更。 該變更表示虛擬網域控制站已還原至先前狀態。 *<COMPUTERNAME>* 會執行下列作業，以防止還原的網域控制站避免可能的資料分歧，以及防止建立具有重複 Sid 的安全性主體：<p>建立新的呼叫識別碼<p>使目前的 RID 集區無效<p>下次執行輸入複寫時將驗證 FSMO 角色的擁有權。 在此期間，若網域控制站持有 FSMO 角色，該角色將無法使用。<p>啟動 SYSVOL 複寫服務還原作業。<p>啟動複寫將還原的網域控制站維持在最新的狀態。<p>要求新的 RID 集區。 |
| **注意事項與解決方法** | 預期會在還原快照時發生。 這會說明將在安全還原過程中發生的所有各種重設作業。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2205** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | *<COMPUTERNAME>* 將虛擬網域控制站還原至先前狀態之後，目前的 RID 集區無效。 |
| **注意事項與解決方法** | 預期會在還原快照時發生。 必須摧毀本機 RID 集區，因為網域控制站已歷經一段時間且可能已經發行這些 RID。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2206** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | ERROR |
| **Message** | *<COMPUTERNAME>* 將虛擬網域控制站還原至先前狀態之後，無法使目前的 RID 集區失效。<p>其他資料：<p>錯誤碼：%1<p>錯誤值：%2 |
| **注意事項與解決方法** | 檢查 [目錄服務] 和 [系統] 事件記錄檔。 驗證 RID 主機已上線，而且可以使用 Dcdiag.exe /test:ridmanager，從這台伺服器連線。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2207** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | ERROR |
| **Message** | *<COMPUTERNAME>* 虛擬網域控制站還原為先前的狀態之後，無法還原。 已要求重新開機到 DSRM。 請檢查先前的事件以取得詳細資訊。 |
| **注意事項與解決方法** | 檢查 [目錄服務] 和 [系統] 事件記錄檔。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2208** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 資訊 |
| **Message** | *<COMPUTERNAME>* 在非權威還原期間刪除 DFSR 資料庫以初始化 SYSVOL 複本。 |
| **注意事項與解決方法** | 預期會在還原快照時發生。 這保證 DFSR 可以從協力電腦 DC 以非權威方式同步處理 SYSVOL。 請注意，SYSVOL 所在的相同磁碟區上任何其他的 DFSR 複寫資料夾也會以非權威方式進行同步處理 (不建議使用網域控制站在 SYSVOL 所在的相同磁碟區上裝載自訂 DFSR 集)。 |

| 事件 | 描述 |
|--|--|
| **事件識別碼** | **2209** |
| **Source** | Microsoft-Windows-ActiveDirectory_DomainService |
| **嚴重性** | 錯誤 |
| **Message** | *<COMPUTERNAME>* 無法刪除 DFSR 資料庫。<p>其他資料：<p>錯誤碼：%1<p>錯誤值：%2<p>Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 *<COMPUTERNAME>* 必須在本機 SYSVOL 複本上初始化非權威還原。 對於 DFSR，若要這樣做，您必須停止 DFSR 服務、刪除 DFSR 資料庫，然後重新啟動該服務。 在重新啟動時，DFSR 將重建資料庫並開始進行初始同步。 |
| **注意事項與解決方法** | 檢查 [DFSR] 事件記錄檔 |

#### <a name="error-messages"></a>錯誤訊息
沒有任何與失敗的虛擬網域控制站安全快照還原直接相關的互動式錯誤；所有的複製資訊都會記錄於 [目錄服務] 事件記錄檔中。 當然，任何重要的複寫或伺服器通告錯誤都會以徵兆的形式在其他位置顯示。

#### <a name="known-issues-and-support-scenarios"></a>已知的問題和支援案例
[疑難排解網域控制站安全還原的一般方法](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore)，通常都足以疑難排解大部分的問題。

|**問題**|**無法在最近安全還原的網域控制站上建立新的安全性主體**|
| -- |--|
|**徵兆**|還原快照之後，嘗試在該網域控制站上建立新的安全性主體 (使用者、電腦、群組) 失敗，並產生：<p>錯誤 0x2010<p>目錄服務無法配置相關的識別碼。|
|**解決方式和注意事項**|這個問題是因為還原的電腦所擁有的 RID 主機 FSMO 角色資訊已過時而導致。 如果在取得快照之後將角色移至這個或其他網域控制站，然後稍後進行還原，還原的網域控制站在初始複寫完成之前都不會具備 RID 主機的資訊。<p>若要解決這個問題，可允許 AD 複寫完成還原網域控制站的輸入。 如果仍然無法運作，請驗證所有的網域控制站都具備哪個 DC 裝載 RID 主機的相同正確資訊。|

|**問題**|**還原的網域控制站不會共用 SYSVOL 通告**|
| -- |--|
|**徵兆**|還原快照後，有一或多個 DC 不會進行通告、不會共用 sysvol，而且沒有最新狀態的 SYSVOL 內容|
|**解決方式和注意事項**|DC 的上游協力電腦沒有運作中的 SYSVOL 複本，能夠使用 DFSR 或 FRS 正確進行複寫。 這個問題與安全還原無關，但很可能會顯示為安全還原問題，因為客戶並未意識到其他會影響非還原 DC 的複寫問題。|

### <a name="advanced-troubleshooting"></a>進階疑難排解
這個單元會使用「處理中」  的記錄檔做為範例，並說明發生的內容，以教導使用者進行進階疑難排解。 如果您了解什麼是成功的虛擬化網域控制站作業，便很容易察覺環境中的失敗。 這些記錄檔是以來源呈現，會以每個記錄檔中複製網域控制站相關的「預期」  事件的遞增順序來顯示。

#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>使用 DFSR 還原複寫 SYSVOL 的網域控制站

##### <a name="directory-services-event-log"></a>目錄服務事件記錄檔
[目錄服務] 記錄檔包含大部分的安全還原作業資訊。 Hypervisor 會變更 VM 世代識別碼，而 NTDS 服務會記錄它，接著使 RID 集區失效，並變更呼叫識別碼。 已設定新的 VM 世代識別碼，而伺服器會複寫 AD 資料輸入。 DFSR 服務已停止，並已刪除裝載 SYSVOL 的資料庫，強制執行輸入的非權威同步處理。 已調整 USN 上限標準。

| **事件識別碼** | **Source** | **Message** |
|--|--|--|
| **2170** | ActiveDirectory_DomainService | 偵測到世代識別碼變更。<p>在 DS 快取的世代識別碼 (舊值)：<p>*<number>*<p>VM 中目前的世代識別碼 (新值)：<p>*<number>*<p>在使用虛擬機器快照後、虛擬機器匯入作業後或即時移轉作業後會發生世代識別碼變更。 Active Directory 網域服務將建立新的呼叫識別碼以復原網域控制站。 虛擬的網域控制站不應使用虛擬機器快照來還原。 要還原或復原 Active Directory 網域服務資料庫的內容，支援的方法是還原以 Active Directory 網域服務感知備份應用程式建立的系統狀態備份。」 |
| **2181** | ActiveDirectory_DomainService | 交易已中止，因為虛擬機器正在還原成先前的狀態。 在使用虛擬機器快照後、虛擬機器匯入作業後或即時移轉作業後發生。 |
| **2204** | ActiveDirectory_DomainService | Active Directory 網域服務偵測到虛擬機器世代識別碼已變更。 該變更表示虛擬網域控制站已還原至先前狀態。 Active Directory 網域服務將執行下列作業，以免已還原的網域控制站發生資料不一致或建立具有重複 SID 的安全性主體：<p>建立新的呼叫識別碼<p>使目前的 RID 集區無效<p>下次執行輸入複寫時將驗證 FSMO 角色的擁有權。 在此期間，若網域控制站持有 FSMO 角色，該角色將無法使用。<p>啟動 SYSVOL 複寫服務還原作業。<p>啟動複寫將還原的網域控制站維持在最新的狀態。<p>要求新的 RID 集區。」 |
| **2181** | ActiveDirectory_DomainService | 交易已中止，因為虛擬機器正在還原成先前的狀態。 在使用虛擬機器快照後、虛擬機器匯入作業後或即時移轉作業後發生。 |
| **1109** | ActiveDirectory_DomainService | 這個目錄伺服器的 invocationID 屬性已變更。 在建立備份時最高的更新序號如下：<p>InvocationID 屬性 (舊值)：<p>*<GUID>*<p>InvocationID 屬性 (新值)：<p>*<GUID>*<p>更新序號：<p>*<number>*<p>當目錄伺服器從備份媒體還原、設定為裝載可寫入的應用程式目錄分割，或在套用虛擬機器快照後、虛擬機器匯入作業後或即時移轉作業後恢復執行時，invocationID 即會變更。 虛擬的網域控制站不應使用虛擬機器快照來還原。 要還原或復原 Active Directory 網域服務資料庫的內容，支援的方法是還原以 Active Directory 網域服務感知備份應用程式建立的系統狀態備份。」 |
| **2179** | ActiveDirectory_DomainService | 網域控制站電腦物件的 msDS-GenerationId 屬性已設為下列參數：<p>GenerationID 屬性：<p>*<number>* |
| **2200** | ActiveDirectory_DomainService | Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 Active Directory 網域服務會初始化複寫，使網域控制站維持在最新狀態。 當複寫完成時，將記錄事件 2201。 |
| **2201** | ActiveDirectory_DomainService | Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 Active Directory 網域服務已完成複寫，使網域控制站維持在最新狀態。 |
| **2185** | ActiveDirectory_DomainService | Active Directory 網域服務已停止用於複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務。<p>服務名稱:<p>DFSR<p>Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 Active Directory 網域服務必須在本機 SYSVOL 複本上初始化非權威還原。 若要這樣做，請停止用來複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務，然後使用適當的登錄機碼與值啟動它以觸發還原。 重新啟動 FRS 或 DFSR 服務時，將記錄事件 2187。」 |
| **2208** | ActiveDirectory_DomainService | Active Directory 網域服務已在非授權還原期間刪除 DFSR 資料庫以初始化 SYSVOL 複本。<p>Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 Active Directory 網域服務必須在本機 SYSVOL 複本上初始化非權威還原。 對於 DFSR，若要這樣做，您必須停止 DFSR 服務、刪除 DFSR 資料庫，然後重新啟動該服務。 重新開機 DFSR 時會重建資料庫，並開始初始同步處理。" |
| **2187** | ActiveDirectory_DomainService | Active Directory 網域服務已啟動用於複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務。<p>服務名稱:<p>DFSR<p>Active Directory 偵測到主控網域控制站的虛擬機器已還原至先前狀態。 Active Directory 網域服務必須在本機 SYSVOL 複本上初始化非權威還原。 若要這樣做，請停止用來複寫 SYSVOL 資料夾的 FRS 或 DFSR 服務，然後使用適當的登錄機碼與值啟動它以觸發還原。 " |
| **1587** | ActiveDirectory_DomainService | 這個目錄服務已被還原或被設定為裝載應用程式目錄分割。 因此，它的複寫識別已經變更。 協力電腦已使用舊的識別要求複寫變更。 起始的序號已被調整。<p>對應於下列物件 GUID 的目的地目錄服務已要求起始於某個 USN 的變更，這個 USN 是在從備份媒體還原的本機目錄服務上的 USN 之前。<p>物件 GUID：<p>*<GUID> (<FQDN of partner domain controller>)*<p>還原發生時的 USN：<p>*<number>*<p>因此，已使用下列設定值來設定目的地目錄服務的即時性向量。<p>先前的資料庫 GUID：<p>*<GUID>*<p>先前的物件 USN：<p>*<number>*<p>先前的屬性 USN：<p>*<number>*<p>新的資料庫 GUID：<p>*<GUID>*<p>新的物件 USN：<p>*<number>*<p>新的屬性 USN：<p>*<number>* |

##### <a name="system-event-log"></a>系統事件記錄檔
[系統] 事件記錄檔會記錄再度將離線的虛擬機器上線時發生的機器時間，並與主機時間同步處理。 RID 集區會失效，且會重新啟動 DFSR 或 FRS 服務。

| **事件識別碼** | **Source** | **Message** |
|--|--|--|
| **1** | Kernel-General | 系統時間已變更為 *？ <now>* 從 *<快照集時間/日期>* 。<p>變更原因：應用程式或系統元件變更了時間。 |
| **16654** | Directory-Services-SAM | 帳戶識別碼 (RID) 集區成為無效。 在下列預期情況中可能會發生此問題：<p>1. 從備份還原網域控制站。<p>2. 在虛擬機器上執行的網域控制站會從快照集還原。<p>3. 系統管理員已手動將集區失效。<p>如需詳細資訊，請參閱 <https://go.microsoft.com/fwlink/?LinkId=226247>。 |
| **7036** | 服務控制管理員 | DFS 複寫服務已進入停止狀態。 |
| **7036** | 服務控制管理員 | DFS 複寫服務已進入執行狀態。 |

##### <a name="application-event-log"></a>應用程式事件記錄檔
[應用程式] 事件記錄檔會記下 DFSR 資料庫的停止與啟動。

| **事件識別碼** | **Source** | **Message** |
|--|--|--|
| **103** | ESENT | DFSRs (1360) \\ \\ .\C： \System Volume Information\DFSR\database <em> _ <GUID> </em> \dfsr.db：資料庫引擎已停止實例 (0) 。<p>中途關機：0<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.141、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.016、[12] 0.000、[13] 0.000、[14] 0.000、[15] 0.000。 |
| **102** | ESENT | DFSRs (532) \\ \\ .\C： \System Volume Information\DFSR\database <em> _ <GUID> </em> \dfsr.db： database engine (6.02.8189.0000) 正在啟動新的實例 (0) 。 |
| **105** | ESENT | DFSRs (532) \\ \\ .\C： \System Volume Information\DFSR\database <em> _ <GUID> </em> \dfsr.db：資料庫引擎已啟動新的實例 (0) 。 (時間=0 秒)<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.031、[10] 0.000、[11] 0.000。 |
|  |  | DFSRs (532) \\ \\ .\C： \System Volume Information\DFSR\database <em> _<GUID> </em> \dfsr.db： database engine 建立了新的資料庫 (1、 \\ \\ .\C： \System Volume Information\DFSR\database <em>_ <GUID> </em> \dfsr.db) 。 (時間=0 秒)<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.016、[4] 0.062、[5] 0.000、[6] 0.016、[7] 0.000、[8] 0.000、[9] 0.015、[10] 0.000、[11] 0.000。 |

##### <a name="dfs-replication-event-log"></a>DFS 複寫事件記錄檔
DFSR 服務已停止，並已刪除包含 SYSVOL 的資料庫，強制執行輸入的非權威同步處理。

| **事件識別碼** | **Source** | **Message** |
|--|--|--|
| **1006** | DFSR | DFS 複寫服務正在停止。 |
| **1008** | DFSR | DFS 複寫服務已停止。 |
| **1002** | DFSR | DFS 複寫服務正在啟動。 |
| **1004** | DFSR | DFS 複寫服務已啟動。 |
| **1314** | DFSR | DFS 複寫服務已順利設定偵錯記錄檔。<p>其他資訊：<p>Debug Log File Path： C:\Windows\debug |
| **6102** | DFSR | DFS 複寫服務已順利登錄 WMI 提供者。 |
| **1206** | DFSR | DFS 複寫服務已成功聯繫網域控制站 *<domain controller FQDN>* 以存取設定資訊。 |
| **1210** | DFSR | DFS 複寫服務已順利設定連入複寫要求的 RPC 接聽程式。<p>其他資訊：<p>埠：0 |
| **4614** | DFSR | DFS 複寫服務已初始化位於本機路徑 C:\Windows\SYSVOL\domain 的 SYSVOL，而且正在等候執行初始複寫。 複寫的資料夾會維持在初始同步狀態，直到它和協力電腦完成複寫為止。 如果伺服器已經開始升級至網域控制站，網域控制站會在此問題解決之後才通告並開始運作。 若指定的協力電腦也處於初始同步狀態，或此伺服器或同步協力電腦發生共用違規，也會發生此情形。 若此事件是發生於將 SYSVOL 從檔案複寫服務 (FRS) 移轉至 DFS 複寫時，則只有在解決問題之後才會對外複寫變更。 這會導致此伺服器上的 SYSVOL 資料夾與其他網域控制站不同步。<p>其他資訊：<p>複寫的資料夾名稱： SYSVOL 共用<p>複寫資料夾識別碼： *<GUID>*<p>複寫組名：網域系統磁片區<p>複寫群組識別碼： *<GUID>*<p>成員識別碼： *<GUID>*<p>唯讀：0 |
| **4604** | DFSR | DFS 複寫服務已順利初始化位於本機路徑 C:\Windows\SYSVOL\domain 的 SYSVOL 複寫資料夾。 此成員已完成與協力電腦 dc1.corp.contoso.com 的 SYSVOL 初始同步處理作業。 若要檢查 SYSVOL 共用是否存在，請開啟命令提示視窗，然後輸入 "net share"。<p>其他資訊：<p>複寫的資料夾名稱： SYSVOL 共用<p>複寫資料夾識別碼： *<GUID>*<p>複寫組名：網域系統磁片區<p>複寫群組識別碼： *<GUID>*<p>成員識別碼： *<GUID>*<p>同步夥伴： *<partner domain controller FQDN>* |

#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>使用 FRS 還原複寫 SYSVOL 的網域控制站
在此案例中，會使用 [檔案複寫] 事件記錄檔，而不是 [DFSR] 事件記錄檔。 [應用程式] 事件記錄檔也會寫入不同的 FRS 相關事件。 否則，[目錄服務] 和 [系統] 事件記錄檔訊息通常是相同的，而且會以先前所述的相同順序出現。

##### <a name="file-replication-service-event-log"></a>檔案複寫服務事件記錄檔
FRS 服務已停止，並使用 D2 BURFLAGS 值重新啟動，以非權威方式同步處理 SYSVOL。

| **事件識別碼** | **Source** | **Message** |
|--|--|--|
| **13502** | NTFRS | 檔案複寫服務正在停止。 |
| **13503** | NTFRS | 檔案複寫服務已停止。 |
| **13501** | NTFRS | 檔案複寫服務正在啟動 |
| **13512** | NTFRS | 檔案複寫服務已在電腦 DC4 上內含目錄 c:\windows\ntfrs\jet 的磁碟偵測出磁碟寫入快取已啟用。 如果磁碟電源中斷而且關鍵的更新遺失，檔案複寫服務可能無法恢復。 |
| **13565** | NTFRS | 檔案複寫服務正在使用其他網域控制站的資料來初始化系統磁碟區。 除非這個處理程序完成，否則電腦 DC4 無法變成網域控制站。 系統磁碟區接著會被視為 SYSVOL 加以共用。<p>如果要檢查 SYSVOL 共用，請在命令提示字元下輸入：<p>net share<p>當檔案複寫服務完成初始化程序，SYSVOL 共用將會出現。<p>系統磁碟區初始化會花費一些時間。 花費的時間視系統磁碟區資料量、其他網域控制站可用性，以及網域控制站之間的複寫間隔而定。」 |
| **13520** | NTFRS | 檔案複寫服務將預先存在的檔案移 *<path>* 至 *<path>* \ NtFrs_PreExisting___See_EventLog。<p>檔案複寫服務可能會隨時刪除 \ NtFrs_PreExisting___See_EventLog 中的檔案 *<path>* 。 您可以將檔案複製到 \ NtFrs_PreExisting___See_EventLog，以從刪除中儲存檔案 *<path>* 。 *<path>* 如果檔案已存在於其他複寫夥伴上，將檔案複製到可能會導致名稱衝突。<p>在某些情況下，檔案複寫服務可能會將檔案從 *<path>* \ NtFrs_PreExisting___See_EventLog 複製到， *<path>* 而不是從其他複寫的夥伴複製檔案。<p>您可以藉由刪除 \ NtFrs_PreExisting___See_EventLog 中的檔案，隨時復原空間 *<path>* 。 |
| **13553** | NTFRS | 檔案複寫服務成功地將這台電腦加入下列複本集：<p>"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<p>這個事件的相關資訊顯示在下方：<p>電腦 DNS 名稱是 " *<domain controller FQDN>* "<p>複本集成員名稱是 " *<domain controller name>* "<p>複本集根路徑是 " *<path>* "<p>複本臨時目錄路徑為 " *<path>* "<p>複本工作目錄路徑是 " *<path>* " |
| **13554** | NTFRS | 檔案複寫服務成功地將下列連線加入複本集中：<p>"DOMAIN SYSTEM VOLUME (SYSVOL SHARE)"<p>從 "" 的輸入 *<partner domain controller FQDN>*<p>輸出到 " *<partner domain controller FQDN>* "<p>後續的事件記錄檔訊息中會有其他的資訊。 |
| **13516** | NTFRS | 檔案複寫服務不會再妨礙電腦 DC4 成為網域控制站。 系統磁碟區已經成功地初始化，而且已通知 Netlogon 服務，系統磁碟區已準備共用為 SYSVOL。<p>請輸入 " net share" 來檢查 SYSVOL 共用。 |

##### <a name="application-event-log"></a>應用程式事件記錄檔
FRS 資料庫停止和啟動，並且會因為 D2 BURFLAGS 作業而加以清除。

| **事件識別碼** | **Source** | **Message** |
|--|--|--|
| **327** | ESENT | ntfrs (1424) 資料庫引擎已中斷連結資料庫 (1，c:\windows\ntfrs\jet\ntfrs.jdb)。 (時間=0 秒)<p>內部計時順序：[1] 0.000、[2] 0.015、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.516、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.063、[12] 0.000。<p>恢復快取：0 |
| **103** | ESENT | ntfrs (1424) 資料庫引擎已停止執行個體 (0)。<p>中途關機：0<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.031、[10] 0.000、[11] 0.016、[12] 0.000、[13] 0.000、[14] 0.047、[15] 0.000。 |
| **102** | ESENT | ntfrs (3000) 資料庫引擎 (6.02.8189.0000) 正在啟動新的執行個體 (0)。 |
| **105** | ESENT | ntfrs (3000) 資料庫引擎啟動了新的執行個體 (0)。 (時間=0 秒)<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.062、[10] 0.000、[11] 0.141。 |
| **103** | ESENT | ntfrs (3000) 資料庫引擎已停止執行個體 (0)。<p>中途關機：0<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.000、[12] 0.000、[13] 0.015、[14] 0.000、[15] 0.000。 |
| **102** | ESENT | ntfrs (3000) 資料庫引擎 (6.02.8189.0000) 正在啟動新的執行個體 (0)。 |
| **105** | ESENT | ntfrs (3000) 資料庫引擎啟動了新的執行個體 (0)。 (時間=0 秒)<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.078、[10] 0.000、[11] 0.109。 |
| **325** | ESENT | ntfrs (3000) 資料庫引擎已建立新的資料庫 (1，c:\windows\ntfrs\jet\ntfrs.jdb)。 (時間=0 秒)<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.016、[4] 0.016、[5] 0.000、[6] 0.015、[7] 0.000、[8] 0.000、[9] 0.078、[10] 0.016、[11] 0.000。 |
| **103** | ESENT | ntfrs (3000) 資料庫引擎已停止執行個體 (0)。<p>中途關機：0<p>內部計時順序：[1] 0.000、[2] 0.000、[3] 0.000、[4] 0.000、[5] 0.078、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.125、[10] 0.016、[11] 0.000、[12] 0.000、[13] 0.000、[14] 0.000、[15] 0.000。 |
| **102** | ESENT | ntfrs (3000) 資料庫引擎 (6.02.8189.0000) 正在啟動新的執行個體 (0)。 |
| **105** | ESENT | ntfrs (3000) 資料庫引擎啟動了新的執行個體 (0)。 (時間=0 秒)<p>內部計時順序：[1] 0.016、[2] 0.000、[3] 0.000、[4] 0.094、[5] 0.000、[6] 0.000、[7] 0.000、[8] 0.000、[9] 0.032、[10] 0.000、[11] 0.000。 |
| **326** | ESENT | ntfrs (3000) 資料庫引擎已連接資料庫 (1，c:\windows\ntfrs\jet\ntfrs.jdb)。 (時間=0 秒)<p>內部計時順序：[1] 0.000、[2] 0.015、[3] 0.000、[4] 0.000、[5] 0.016、[6] 0.015、[7] 0.000、[8] 0.000、[9] 0.000、[10] 0.000、[11] 0.000、[12] 0.000。<p>已儲存快取：1 |
