---
ms.assetid: 249ba1be-b0d3-4a77-99af-3699074a2b6e
title: "擬化檔案網域控制站疑難排解"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 63f96e7a3035bc25f7a7a349acb6bf26f62a2a3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-troubleshooting"></a>擬化檔案網域控制站疑難排解

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供疑難排解模擬的網域控制站功能詳細的方法。  
  
-   [網域控制站複製疑難排解擬化檔案](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCCloning)  
  
-   [疑難排解模擬的網域控制站安全還原](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootVDCSafeRestore)  
  
## <a name="BKMK_Intro"></a>簡介  
以改善您的疑難排解技術最重要的方式是實驗室測試的組建，並嚴格檢查 [正常運作的案例。 如果您遇到錯誤，有更多明顯並了解，因為您，並需要實心基本知識網域控制站升級的運作方式的簡單。 這也可讓您建置您分析及網路分析技巧。 這會所有分散式的系統技術，不只是模擬的網域控制站部署。  
  
重要進階疑難排解網域控制站設定的項目︰  
  
1.  線性分析加上焦，並注意詳細資料。  
  
2.  了解網路擷取分析  
  
3.  了解建登  
  
第一次並第二種超出範圍本主題中，但第三個可以將某些詳細資料所述。 模擬的網域控制站疑難排解需要邏輯和線性的方法。 關鍵在於接近使用所提供的資料的問題，以及您已登入與提供的輸出用完時，只信複雜的工具和分析。  
  
## <a name="BKMK_TshootVDCCloning"></a>網域控制站複製疑難排解擬化檔案  
這區段鍵盤保護蓋：  
  
-   [疑難排解工具](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_Tools)  
  
-   [登入選項](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_LoggingOptions)  
  
-   [一般方法，以取得疑難排解網域控制站複製](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)  
  
-   [伺服器 Core 和事件登入](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_ServerCoreEvents)  
  
-   [特定的問題進行疑難排解](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_SpecificProblems)  
  
疑難排解策略模擬的網域控制站複製依照此一般格式：  
  
![virtual 俠疑難排解](media/Virtualized-Domain-Controller-Troubleshooting/ADDS_VDC_TroublehsootingFlowchart.png)  
  
### <a name="BKMK_Tools"></a>疑難排解工具  
  
#### <a name="BKMK_LoggingOptions"></a>登入選項  
建登的最重要的網域控制站複製問題的疑難排解工具。 所有的這些登的功能和最大的詳細資訊，預設設定。  
  
|||  
|-|-|  
|**操作**|**登入**|  
|**複製**|事件 viewer\Windows logs\System<br />事件 viewer\Applications 和服務 logs\Directory 服務<br />-%systemroot%\debug\dcpromo.log|  
|**促銷**|-%systemroot%\debug\dcpromo.log<br />事件 viewer\Applications 和服務 logs\Directory 服務<br />事件 viewer\Windows logs\System<br />事件 viewer\Applications 和服務 logs\File 複寫服務<br />事件 viewer\Applications 和服務 logs\DFS 複寫|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>工具和疑難排解網域控制站設定的命令  
若要登不解釋問題的疑難排解，使用下列工具做為起點：  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   網路監視器 3.4  
  
### <a name="BKMK_GeneralMethodology"></a>一般方法，以取得疑難排解網域控制站複製  
  
1.  VM 開機至 DS 修復模式 (DSRM)？ 這表示疑難排解必要。 若要在 DSRM 登入，請使用**。 \Administrator**帳號，並在指定 DSRM 密碼。  
  
    1.  檢查 Dcpromo.log。  
  
        1.  是否初始複製步驟成功，但無法網域控制站升級？  
  
        2.  錯誤指定本機網域控制站或 AD DS 環境中，使用的問題，例如從肯定傳回錯誤？  
  
    2.  檢查 [系統和 Directory 服務事件登 dccloneconfig.xml 和 CustomDCCloneAllowList.xml  
  
        1.  不相容應用程式需要是 CustomDCCloneAllowList.xml 允許清單？  
  
        2.  複製或不正確 dccloneconfig.xml 中 IP 位址或電腦名稱是？  
  
        3.  無效的 Active Directory 網站 dccloneconfig.xml 中嗎？  
  
        4.  IP 位址未設定 dccloningconfig.xml 中，有可用的任何 DHCP 伺服器嗎？  
  
        5.  而且肯定 online 可透過 RPC 通訊協定嗎？  
  
        6.  複製網域控制站群組成員為網域控制站嗎？ 權限**允許建立自己的複本 DC**設定為網域根嗎？  
  
        7.  Dccloneconfig.xml 檔案是否包含避免正確剖析語法錯誤？  
  
        8.  Hypervisor 支援？  
  
        9. 複製開始成功後，是否網域控制站促銷失敗？  
  
        10. 已自動自訂網域控制站名稱 (9999) 超過人數嗎？  
  
        11. 重複的 MAC 地址？  
  
2.  做為來源俠相同是複製主機名稱嗎？  
  
    1.  是否有 Dccloneconfig.xml 檔案中的其中一個允許的位置？  
  
3.  VM 開機進入標準模式和複製完成後，但無法正確運作的網域控制站嗎？  
  
    1.  第一次，請檢查是否主機名稱變更上複製。 如果不同的主機名稱，請複製最少部分完成。  
  
    2.  網域控制站是否有來源網域控制站 dccloneconfig.xml，從重複的 IP 位址，但期間複製已離線來源網域控制站？  
  
    3.  如果您的網域控制站廣告，視為您不需要複製會有任何一般後促銷問題問題。  
  
    4.  如果您的網域控制站不廣告，檢查 Directory 服務、 系統、 應用程式、 檔案複寫和 DFS 複寫事件登後升級的錯誤。  
  
#### <a name="disabling-dsrm-boot"></a>停用 DSRM 開機  
一旦開機進入 DSRM 任何錯誤，因為診斷失敗的原因及 dcpromo.log 不會指出的複製無法重試，如果修正失敗的原因 DSRM 旗標重設。 失敗的複製不會傳回至標準模式自己在下一步重新開機。您必須以再試一次複製移除 DS 還原模式開機旗標。 所有的這些步驟會需要以提升權限的系統管理員身分執行。  
  
##### <a name="removing-dsrm-with-msconfigexe"></a>移除 DSRM Msconfig.exe 使用  
若要關閉使用 GUI DSRM 開機，請使用 「 系統設定工具：  
  
1.  執行 msconfig.exe  
  
2.  在**開機**索引標籤，在**開機選項**，取消選取**安全開機**(它已選取的選項**Active Directory 修復**功能)  
  
3.  按一下 [確定] 並重新出現提示時  
  
##### <a name="removing-dsrm-with-bcdeditexe"></a>移除 DSRM Bcdedit.exe 與  
若要關閉 DSRM 開機從命令列，請使用開機設定資料儲存區編輯器：  
  
1.  打開 CMD 提示並執行：  
  
    ```  
    Bcdedit.exe /deletevalue safeboot  
    ```  
  
2.  重新播放的電腦：  
  
    ```  
    Shutdown.exe /t /0 /r  
    ```  
  
> [!NOTE]  
> Bcdedit.exe 也適用於 Windows PowerShell 主機。 有命令：  
>   
> Bcdedit.exe /deletevalue 安全開機  
>   
> 電腦重新開機  
  
### <a name="BKMK_ServerCoreEvents"></a>伺服器 Core 和事件登入  
事件登包含許多關於模擬的網域控制站複製作業有用的資訊。 根據預設，Windows Server 2012 的電腦安裝是 Server Core 安裝，這表示不圖形介面，因此，不執行本機事件檢視器嵌入式管理單元的方式。  
  
若要檢視事件登執行 Server Core 安裝的伺服器上：  
  
-   在本機上執行的 Wevtutil.exe 工具  
  
-   在本機上執行 PowerShell cmdlet 取得-WinEvent  
  
-   如果您有支援 Windows 進階免 「 事件登入遠端管理 」 群組 （或相當於連接埠） 允許輸入的通訊，您可以管理從遠端使用 Eventvwr.exe、 wevtutil.exe 或取得-Winevent 事件登入。 這可以使用 Windows PowerShell 3.0 NETSH.exe、 群組原則或設定為 NetFirewallRule cmdlet 新 Server Core 安裝完成。  
  
> [!WARNING]  
> 請勿在 DSRM 時，圖形殼層新增到電腦。 Windows 維護堆疊 (CBS) 無法正常運作，同時在安全模式 」 或 「 DSRM。 若要新增的功能或在 DSRM 中的角色嘗試而不完成，而且的電腦保持在不穩定的狀態，直到它通常會開機。 因為在 DSRM 模擬的網域控制站複製一樣，無法開機，且應該不會開機通常會在大部分的環境，很難安全地新增的圖形殼層。 如此一來不支援，可讓您無法使用的伺服器。  
  
### <a name="BKMK_SpecificProblems"></a>特定的問題進行疑難排解  
  
#### <a name="events"></a>事件  
所有模擬的網域控制站複製事件寫入複製網域控制站 VM Directory 服務事件登入。 應用程式、 檔案複寫服務及 DFS 複寫事件登也可能包含失敗複製實用疑難排解資訊。 以肯定 RPC 通話期間失敗可能會提供肯定事件登入。  
  
以下是 Windows Server 2012 複製特定 Directory 服務事件登入，資訊與建議的解析度的錯誤事件。  
  
##### <a name="directory-services-event-log"></a>Directory 服務事件登入  
  
|||  
|-|-|  
|**事件編號**|**2160**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|本機* <COMPUTERNAME> *找到 virtual 網域控制站複製設定檔。<br /><br />在找到 virtual 網域控制站複製設定檔： %1<br /><br />差 virtual 網域控制站複製設定檔表示本機 virtual 網域控制站其他 virtual 網域控制站的複本。 * <COMPUTERNAME> *複製本身將會開始。|  
|**筆記與解析度**|如果未預期，是成功的事件和的問題。 檢查 DSA 運作 Directory、 %systemroot%\ntds 和根的任何本機或卸除式磁碟 dcclconeconfig.xml 檔案。|  
  
|||  
|-|-|  
|**事件編號**|**2161**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|本機* <COMPUTERNAME> *找不到 virtual 網域控制站複製設定檔。 不複製的 DC 本機電腦。|  
|**筆記與解析度**|如果未預期，是成功的事件和的問題。 檢查 DSA 運作 Directory、 %systemroot%\ntds 和根的任何本機或卸除式磁碟 dcclconeconfig.xml 檔案。|  
  
|||  
|-|-|  
|**事件編號**|**2162**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|Virtual 網域控制站複製失敗。<br /><br />請登入系統事件登和 %systemroot%\debug\dcpromo.log 如需詳細資訊，在 [對應至複製嘗試 virtual 網域控制站的錯誤事件。<br /><br />錯誤碼： %1|  
|**筆記與解析度**|遵循訊息的指示，這項錯誤會 catchall。|  
  
|||  
|-|-|  
|**事件編號**|**2163**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|DsRoleSvc 服務已複製本機 virtual 網域控制站開始。|  
|**筆記與解析度**|如果未預期，是成功的事件和的問題。 檢查 DSA 運作 Directory、 %systemroot%\ntds 和根的任何本機或卸除式磁碟 dcclconeconfig.xml 檔案。|  
  
|||  
|-|-|  
|**事件編號**|**2164**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*若要開始 DsRoleSvc 服務複製本機 virtual 網域控制站失敗。|  
|**筆記與解析度**|檢查服務 DS 角色伺服器服務 (DsRoleSvc) 的設定，並確定其開始輸入設定為手動。 驗證無第三方程式造成 [開始] 的這項服務。|  
  
|||  
|-|-|  
|**事件編號**|**2165**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*開始在本機 virtual 網域控制站複製的討論串中有失敗。<br /><br />錯誤碼: %1<br /><br />錯誤訊息: %2<br /><br />執行緒 3%名稱：|  
|**筆記與解析度**|連絡 Microsoft Product 支援|  
  
|||  
|-|-|  
|**事件編號**|**2166**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*需要起始重新開機一次插入 DSRM RPCSS 服務。 等待 RPCSS 初始化失敗執行的狀態。<br /><br />錯誤碼: %1|  
|**筆記與解析度**|檢查服務 RPC 伺服器服務 (Rpcss) 的設定和系統事件登入|  
  
|||  
|-|-|  
|**事件編號**|**2167**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*無法初始化 virtual 網域控制站知識。 查看先前事件登入的項目，如需詳細資訊。<br /><br />其他資料<br /><br />失敗碼: %1|  
|**筆記與解析度**|遵循訊息的指示，這項錯誤會 catchall。|  
  
|||  
|-|-|  
|**事件編號**|**2168**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|Microsoft-Windows-ActiveDirectory_DomainService<br /><br />DC 支援 hypervisor 上執行。 偵測到 VM 代來電顯示。<br /><br />目前值 VM 代 ID: %1|  
|**筆記與解析度**|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|**事件編號**|**2169**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|不還有偵測到的任何 VM 代來電顯示。 實體機器、 向下層級的版本 HYPER-V，或不支援 VM 代 ID hypervisor 裝載 DC<br /><br />其他資料<br /><br />失敗碼檢查 VM 代 %1 時傳回|  
|**筆記與解析度**|如果您不打算複製，是成功事件。 否則，請檢查 [系統事件登入並檢視 hypervisor product 支援文件。|  
  
|||  
|-|-|  
|**事件編號**|**2170**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|警告|  
|**訊息**|偵測到代 ID 變更。<br /><br />代來電顯示快取中 DS （舊值）: %1<br /><br />代 ID 目前在 VM （新值）: %2<br /><br />一樣快照的應用程式、 一樣匯入操作後或即時移轉作業之後，就會發生代 ID 變更。 *<COMPUTERNAME>*將會建立新的叫用 ID 復原網域控制站。 應還原模擬的網域控制站使用一樣的快照。 支援的方法來還原或復原 content 的 Active Directory Domain Services 資料庫是還原所做的 Active Directory Domain Services 注意備份應用程式的系統狀態備份。|  
|**筆記與解析度**|如果您要複製，這是成功事件。 否則，請檢查系統事件登入。|  
  
|||  
|-|-|  
|**事件編號**|**2171**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|偵測不代 ID 變更。<br /><br />代來電顯示快取中 DS （舊值）: %1<br /><br />代 ID 目前在 VM （新值）: %2|  
|**筆記與解析度**|如果您不打算複製，是成功的事件和應該會出現在模擬 DC 重新開機。 否則，請檢查系統事件登入。|  
  
|||  
|-|-|  
|**事件編號**|**2172**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|朗讀 msDS GenerationId 網域控制站的電腦物件的屬性。<br /><br />msDS-GenerationId 屬性的值: %1|  
|**筆記與解析度**|如果您要複製，這是成功事件。 否則，請檢查系統事件登入。|  
  
|||  
|-|-|  
|**事件編號**|**2173**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|無法讀取 msDS-GenerationId 網域控制站的電腦物件的屬性。 這可能因為資料庫交易失敗，或在本機資料庫代 id 不存在。 之後帶領或俠不 virtual 網域控制站 msDS GenerationId 不存在期間的第一次重新開機。<br /><br />其他資料<br /><br />失敗碼: %1|  
|**筆記與解析度**|這是成功事件，如果您要複製並在第一次 VM 重新開機之後並複製已完成。 也可以在非 virtual 網域控制站忽略它。 否則，請檢查系統事件登入。|  
  
|||  
|-|-|  
|**事件編號**|**2174**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|DC 是 virtual 網域控制站複製都還原 virtual 網域控制站開發進程的快照。|  
|**筆記與解析度**|如果您不打算複製，是成功事件。 否則，請檢查系統事件登入。|  
  
|||  
|-|-|  
|**事件編號**|**2175**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|支援的平台上，有 virtual 網域控制站複製設定檔。|  
|**筆記與解析度**|這是時找到 dccloneconfig.xml VM 代編號找不到，例如所在的電腦上或不支援 VM 代 ID hypervisor 時找到的 dccloneconfig.xml 檔案|  
  
|||  
|-|-|  
|**事件編號**|**2176**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|重新命名 virtual 網域控制站複製設定檔。<br /><br />其他資料<br /><br />舊檔案名稱: %1<br /><br />新檔案名稱: %2|  
|**筆記與解析度**|如預期般時開機來源 VM 備份，因為已經不會變更 VM 代 ID 重新命名。 如此可防止來源網域控制站嘗試複製。|  
  
|||  
|-|-|  
|**事件編號**|**2177**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|重新命名 virtual 網域控制站複製設定檔失敗。<br /><br />其他資料<br /><br />檔案名稱: %1<br /><br />失敗碼: %%32|  
|**筆記與解析度**|重新命名嘗試時預期的來源 VM 開機備份，因為 VM 代 ID 已經不會變更。 如此可防止來源網域控制站嘗試複製。 手動重新命名檔案，並調查安裝第三方你，可能會阻止重新命名檔案。|  
  
|||  
|-|-|  
|**事件編號**|**2178**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|偵測到 virtual 網域控制站複製設定檔案，但 VM 代 ID 不已變更。 本機俠是俠複製來源。 重新命名複製設定檔。|  
|**筆記與解析度**|時，有開機來源 VM 備份，因為已經不會變更 VM 代來電顯示。 如此可防止來源網域控制站嘗試複製。|  
  
|||  
|-|-|  
|**事件編號**|**2179**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|MsDS-GenerationId 物件的屬性網域控制站的電腦已設定為下列參數：<br /><br />GenerationID 屬性: %1|  
|**筆記與解析度**|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|**事件編號**|**2180**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|警告|  
|**訊息**|無法將 msDS-GenerationId 網域控制站的電腦物件的屬性。<br /><br />其他資料<br /><br />失敗碼: %1|  
|**筆記與解析度**|檢查 Dcpromo.log 和系統事件登入。 查詢特定 MS TechNet、 MS 微軟，與 MS 部落格，以判斷其一般還久的意義，以及然後疑難排解錯誤會根據這些結果。|  
  
|||  
|-|-|  
|**事件編號**|**2182**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|事件內部： 被要求複製遠端 DSA Directory 服務：|  
|**筆記與解析度**|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|**事件編號**|**2183**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|事件內部： * <COMPUTERNAME> *完成要求複製遠端 Directory 系統代理程式。<br /><br />原始俠名稱: %3<br /><br />要求複製俠名稱: %4<br /><br />要求複製 DC 網站: %5<br /><br />其他資料<br /><br />錯誤: %值 1 %2|  
|**筆記與解析度**|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|**事件編號**|**2184**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*建立網域控制站 account 複製俠失敗。<br /><br />原始俠名稱: %1<br /><br />允許的數目複製 DC:%2<br /><br />上的 [由複製的網域控制站帳號數限制* <COMPUTERNAME>*超過。|  
|**筆記與解析度**|如果網域控制站不會降級，根據命名規格單一來源網域控制站名稱只會自動產生 9999 時間。 使用<computername>的不同名稱 DC 產生新的唯一名稱或複製 XML 中的項目。|  
  
|||  
|-|-|  
|**事件編號**|**2191**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|*<COMPUTERNAME>*若要停用 DNS 更新下列登錄值設成。<br /><br />登錄鍵: %1<br /><br />登錄值： %2<br /><br />登錄數值資料： %3<br /><br />複製程序，在本機電腦可能會有一小段時間複製來源電腦相同電腦的名稱。 DNS A AAAA 記錄登記會停用在這段期間，因此戶端找不到本機電腦目前正在進行複製傳送要求。 複製程序將會複製完成後，再試一次讓 DNS 更新。|  
|**筆記與解析度**|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|**事件編號**|**2192**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*將下列登錄值來停用 DNS 更新失敗。<br /><br />登錄鍵: %1<br /><br />登錄值： %2<br /><br />登錄數值資料： %3<br /><br />錯誤碼: %4<br /><br />錯誤訊息: %5<br /><br />複製程序，在本機電腦可能會有一小段時間複製來源電腦相同電腦的名稱。 DNS A AAAA 記錄登記會停用在這段期間，因此戶端找不到本機電腦目前正在進行複製傳送要求。|  
|**筆記與解析度**|檢查應用程式和系統事件登。 調查第三方應用程式可能會封鎖登錄更新。|  
  
|||  
|-|-|  
|**事件編號**|**2193**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|*<COMPUTERNAME>*設定，可讓 DNS 更新登錄下列值。<br /><br />登錄鍵: %1<br /><br />登錄值： %2<br /><br />登錄數值資料： %3<br /><br />複製程序，在本機電腦可能會有一小段時間複製來源電腦相同電腦的名稱。 DNS A AAAA 記錄登記會停用在這段期間，因此戶端找不到本機電腦目前正在進行複製傳送要求。|  
|**筆記與解析度**|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|**事件編號**|**2194**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*下列登錄將值設為讓 DNS 更新失敗。<br /><br />登錄鍵: %1<br /><br />登錄值： %2<br /><br />登錄數值資料： %3<br /><br />錯誤碼: %4<br /><br />錯誤訊息: %5<br /><br />複製程序，在本機電腦可能會有一小段時間複製來源電腦相同電腦的名稱。 DNS A AAAA 記錄登記會停用在這段期間，因此戶端找不到本機電腦目前正在進行複製傳送要求。|  
|**筆記與解析度**|檢查應用程式和系統事件登。 調查第三方應用程式可能會封鎖登錄更新。|  
  
|||  
|-|-|  
|**事件編號**|**2195**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|將開機 DSRM 失敗。<br /><br />錯誤碼: %1<br /><br />錯誤訊息: %2<br /><br />當 virtual 網域控制站複製無法或 virtual 網域控制站複製設定檔會出現在未受支援 hypervisor、 在本機電腦將會到 DSRM 重新開機進行疑難排解。 設定 DSRM 開機失敗。|  
|**筆記與解析度**|檢查應用程式和系統事件登。 調查第三方應用程式可能會封鎖登錄更新。|  
  
|||  
|-|-|  
|**事件編號**|**2196**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|無法讓關機權限。<br /><br />錯誤碼: %1<br /><br />錯誤訊息: %2<br /><br />當 virtual 網域控制站複製無法或 virtual 網域控制站複製設定檔會出現在未受支援 hypervisor、 在本機電腦將會到 DSRM 重新開機進行疑難排解。 讓關閉雲端失敗。|  
|**筆記與解析度**|檢查應用程式和系統事件登。 調查第三方應用程式可能會封鎖權限使用量。|  
  
|||  
|-|-|  
|**事件編號**|**2197**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|無法起始系統關機。<br /><br />錯誤碼: %1<br /><br />錯誤訊息: %2<br /><br />當 virtual 網域控制站複製無法或 virtual 網域控制站複製設定檔會出現在未受支援 hypervisor、 在本機電腦將會到 DSRM 重新開機進行疑難排解。 起始系統關機失敗。|  
|**筆記與解析度**|檢查應用程式和系統事件登。 調查第三方應用程式可能會封鎖權限使用量。|  
  
|||  
|-|-|  
|**事件編號**|**2198**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*無法建立或修改下列複製的 DC 物件。<br /><br />其他資料：<br /><br />物件：<br /><br />%1<br /><br />錯誤值： %2<br /><br />%3|  
|**筆記與解析度**|查詢特定 MS TechNet、 MS 微軟，與 MS 部落格，以判斷其一般還久的意義，以及然後疑難排解錯誤會根據這些結果。|  
  
|||  
|-|-|  
|**事件編號**|**2199**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*無法建立下列複製的 DC 物件，因為已經有物件。<br /><br />其他資料：<br /><br />來源俠：<br /><br />%1<br /><br />物件：<br /><br />%2|  
|**筆記與解析度**|驗證 dccloneconfig.xml 未指定現有的網域控制站或複本 dccloneconfig.xml 有編輯名稱若不使用多個複製上。 如果仍未預期衝突，判斷的系統管理員升級。連絡它們討論是否應該降級現有的網域控制站，接著現有的網域控制站中繼資料，或是複製應該使用不同名稱。|  
  
|||  
|-|-|  
|**事件編號**|**2203**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|複製過去 virtual 網域控制站失敗。 這就是第一個在重新開機之後，然後，這應該是於複製重試。 但是，有兩 virtual 網域控制站複製設定檔，也偵測到一樣代 ID 變更。 DSRM 開機。<br /><br />複製過去 virtual 網域控制站無法: %1<br /><br />有 virtual 網域控制站複製設定檔: %2<br /><br />偵測到一樣代 ID 變更: %3|  
|**筆記與解析度**|如果複製失敗之前，因為遺失或不正確 dccloneconfig.xml 如預期般|  
  
|||  
|-|-|  
|事件編號|2210|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|錯誤|  
|訊息|<COMPUTERNAME> 無法建立複製網域控制站的物件。<br /><br />其他資料：<br /><br />複製 Id: %6<br /><br />複製網域控制站名稱： %1<br /><br />再試一次進度： %2<br /><br />值例外： %3<br /><br />錯誤值： %4<br /><br />DSID: %5|  
|筆記與解析度|檢視系統和 Directory 服務事件登和 dcpromo.log 如需詳細資訊複製失敗的原因。|  
  
|||  
|-|-|  
|事件編號|2211|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|<COMPUTERNAME> 建立複製網域控制站的物件。<br /><br />其他資料：<br /><br />複製 Id: %3<br /><br />複製網域控制站名稱： %1<br /><br />再試一次進度： %2|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2212|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|<COMPUTERNAME> 開始建立網域控制站複製物件。<br /><br />其他資料：<br /><br />複製 Id: %1<br /><br />複製名稱： %2<br /><br />複製網站： %3<br /><br />複製 RODC: %4|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2213|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|<COMPUTERNAME> 建立新的唯讀網域控制站複製 KrbTgt 物件。<br /><br />其他資料：<br /><br />複製 Id: %1<br /><br />新的 KrbTgt 物件 Guid: %2|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2214|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|<COMPUTERNAME> 將會建立複製網域控制站電腦物件。<br /><br />其他資料：<br /><br />複製 Id: %1<br /><br />原始網域控制站： %2<br /><br />複製網域控制站： %3|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2215|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|<COMPUTERNAME> 將會在下列網站中新增複製網域控制站。<br /><br />其他資料：<br /><br />複製 Id: %1<br /><br />網站： %2|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2216|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|<COMPUTERNAME> 將會建立容器複製網域控制站伺服器。<br /><br />其他資料：<br /><br />複製 Id: %1<br /><br />伺服器容器： %2|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2217|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|<COMPUTERNAME> 將會建立複製網域控制站伺服器物件。<br /><br />其他資料：<br /><br />複製 Id: %1<br /><br />伺服器物件： %2|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2218|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|<COMPUTERNAME> 將會建立複製網域控制站 NTDS 設定物件。<br /><br />其他資料：<br /><br />複製 Id: %1<br /><br />物件： %2|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2219|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|<COMPUTERNAME> 將會建立複製唯讀網域控制站連接物件。<br /><br />其他資料：<br /><br />複製 Id: %1|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2220|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|<COMPUTERNAME> 將會建立複製唯讀網域控制站 SYSVOL 物件。<br /><br />其他資料：<br /><br />複製 Id: %1|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2221|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|錯誤|  
|訊息|<COMPUTERNAME> 無法產生隨機複製的網域控制站的密碼。<br /><br />其他資料：<br /><br />複製 Id: %1<br /><br />複製網域控制站名稱： %2<br /><br />錯誤： %3%4|  
|筆記與解析度|檢查 [系統事件登入，如需詳細資訊為何無法建立電腦密碼。|  
  
|||  
|-|-|  
|事件編號|2222|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|錯誤|  
|訊息|<COMPUTERNAME> 無法複製的網域控制站設定密碼。<br /><br />其他資料：<br /><br />複製 Id: %1<br /><br />複製網域控制站名稱： %2<br /><br />錯誤： %3%4|  
|筆記與解析度|檢查 [系統事件登入，如需詳細資訊為何無法設定電腦密碼。|  
  
|||  
|-|-|  
|事件編號|2223|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|<COMPUTERNAME> 複製的網域控制站成功設定電腦密碼。<br /><br />其他資料：<br /><br />複製 Id: %1<br /><br />複製網域控制站名稱： %2<br /><br />總重試次數： %3|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2224|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|錯誤|  
|訊息|Virtual 網域控制站複製失敗。 下列 %1 管理服務帳號存在複製電腦:<br /><br />%2<br /><br />適用於複製成功，必須移除所有管理服務帳號。 這可以使用移除-ADComputerServiceAccount PowerShell cmdlet。|  
|筆記與解析度|使用獨立 MSAs 時的預期 （不群組 MSA）。 執行*未*移除 account 事件建議，請依照下列-不正確撰寫。 使用 Uninstall-AdServiceAccount- [https://technet.microsoft.com/library/hh852310](https://technet.microsoft.com/library/hh852310)。<br /><br />第一次在 Windows Server 2008 R2 推出的-獨立 MSAs 所取代群組 MSAs (gMSA) 與 Windows Server 2012 中。 GMSAs 支援複製。|  
  
|||  
|-|-|  
|事件編號|2225|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|資訊|  
|訊息|下列安全性原則的快取機密資料已順利從本機網域控制站移除：<br /><br />%1<br /><br />複製唯讀網域控制站之後, 機密複製來源唯讀網域控制站之前快取的資料將被移除複製的網域控制站。|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|2226|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|錯誤|  
|訊息|無法從本機網域控制站移除下列安全性原則的快取的密碼：<br /><br />%1<br /><br />錯誤： %2 (%3)<br /><br />之後複製唯讀網域控制站，機密先前已複製降低風險攻擊，可以從遭竊，或危害複製取得認證，以移除上複製來源唯讀網域控制站需要在快取的資料。 如果您的安全性原則高度授權的帳號，並針對這應該受，請使用進行 rootDSE 作業 rODCPurgeAccount 手動清除其可在本機網域控制站。|  
|筆記與解析度|檢查 [系統及 Directory 服務事件登的其他相關資訊。|  
  
|||  
|-|-|  
|事件編號|2227|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|錯誤|  
|訊息|例外嘗試移除本機網域控制站的快取的密碼。<br /><br />其他資料：<br /><br />值例外： %1<br /><br />錯誤值： %2<br /><br />DSID: %3<br /><br />之後複製唯讀網域控制站，機密先前已複製降低風險攻擊，可以從遭竊，或危害複製取得認證，以移除上複製來源唯讀網域控制站需要在快取的資料。 如果這些安全性原則的任何高度授權的帳號，並針對這應該受，請使用進行 rootDSE 作業 rODCPurgeAccount 手動清除其可在本機網域控制站。|  
|筆記與解析度|檢查 [系統及 Directory 服務事件登的其他相關資訊。|  
  
|||  
|-|-|  
|事件編號|2228|  
|來源|Microsoft-Windows-ActiveDirectory_DomainService|  
|嚴重性|錯誤|  
|訊息|在這個網域控制站的 Active Directory 資料庫一樣代 ID 是不同目前此一樣的值。 不過，virtual 網域控制站複製設定檔 (DCCloneConfig.xml) 找不到，網域控制站複製不嘗試。 如果複製作業網域控制站的目的是，請確定該 DCCloneConfig.xml 所提供的其中一個支援的位置。 此外，此網域控制站的 IP 位址和其他網域控制站的 IP 位址衝突。 若要確保發生任何服務中斷，已設定的網域控制站 DSRM 到開機。<br /><br />其他資料：<br /><br />重複的 IP 位址： %1|  
|筆記與解析度|這個保護機制停止重複的網域控制站時可能 （將不時使用 DHCP，例如）。 將有效 DcCloneConfig.xml 檔案新增、 移除 DSRM 旗標和重新嘗試複製|  
  
|||  
|-|-|  
|事件編號|29218|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|Virtual 網域控制站複製失敗。 無法完成複製作業，複製的網域控制站重新開機至 Directory 服務還原模式 (DSRM)。<br /><br />請核取先前登入事件和 %systemroot%\debug\dcpromo.log 錯誤對應的詳細資訊的 virtual 網域控制站複製嘗試和可以重新使用此複製映像。<br /><br />如果有一或多個登入的項目，表示無法重試複製程序，必須確實損壞映像。 否則您可能會修正的錯誤、 清除 DSRM 開機旗標和開機通常;在重新開機，時將會重試複製操作。|  
|筆記與解析度|檢視系統和 Directory 服務事件登和 dcpromo.log 如需詳細資訊複製失敗的原因。|  
  
|||  
|-|-|  
|事件編號|29219|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|資訊|  
|訊息|Virtual 網域控制站複製成功。|  
|筆記與解析度|如果未預期，是成功的事件和的問題。|  
  
|||  
|-|-|  
|事件編號|29248|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|若要取得 Winlogon 通知 virtual 網域控制站複製失敗。 傳回的錯誤碼是 %1 (%2)。<br /><br />如需有關這個錯誤的詳細資訊，請檢查 %systemroot%\debug\dcpromo.log 對應複製嘗試 virtual 網域控制站的錯誤。|  
|筆記與解析度|連絡 Microsoft Product 支援|  
  
|||  
|-|-|  
|事件編號|29249|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|Virtual 網域控制站複製無法剖析 virtual 網域控制站設定檔。<br /><br />傳回的 HRESULT 程式碼是 %1。<br /><br />設定檔是： 2%<br /><br />請設定檔中修正的錯誤，然後再試一次複製作業。<br /><br />如需有關這個錯誤，請查看 %systemroot%\debug\dcpromo.log。|  
|筆記與解析度|檢查使用 XML 編輯器語法錯誤 dclconeconfig.xml 檔案和 DCCloneConfigSchema.xsd 架構檔案。|  
  
|||  
|-|-|  
|事件編號|29250|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|Virtual 網域控制站複製失敗。 有軟體或複製 virtual 網域控制站在目前支援的服務不會存在 virtual 網域控制站複製允許的應用程式清單中。<br /><br />以下是遺失的項目：<br /><br />%2<br /><br />（如果有的話） %1 用來做定義的包含清單。<br /><br />如果有非複製安裝的應用程式無法完成複製作業。<br /><br />請執行 Active Directory PowerShell Cmdlet 取得-ADDCCloningExcludedApplicationList 以查看哪些應用程式安裝複製在電腦上，但不是包含在允許清單中，並將他們新增到允許清單中，如果有相容 virtual 網域控制站複製。 如果這些應用程式的任何不相容 virtual 網域控制站複製，請將它們解除安裝再重新嘗試複製作業。<br /><br />允許應用程式清單檔案，CustomDCCloneAllowList.xml，依據以下搜尋順序; 搜尋 virtual 網域控制站複製程序使用的第一個檔案，找到和其他所有人會忽略：<br /><br />1.登錄值名稱： HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters\AllowListFolder<br /><br />2.DSA 運作 Directory 資料夾所在的相同 directory<br /><br />3.%windir%\NTDS<br /><br />4.讀取/寫入卸除式媒體順序的磁碟機代號根的磁碟機。|  
|筆記與解析度|請依照下列指示訊息|  
  
|||  
|-|-|  
|事件編號|29251|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|若要重設複製電腦的 IP 位址 virtual 網域控制站複製失敗。<br /><br />傳回的錯誤碼是 %1 (%2)。<br /><br />這個錯誤可能是由錯誤網路設定區段中 virtual 網域控制站設定檔中。<br /><br />請查看 %systemroot%\debug\dcpromo.log 的詳細資訊的 IP 位址期間 virtual 網域控制站複製嘗試重設為等於錯誤。<br /><br />在 https://go.microsoft.com/fwlink/?LinkId=208030 找到重設電腦的 IP 位址複製電腦上的詳細資訊|  
|筆記與解析度|請確認無效，而且不重複的原始的來源電腦設定中 dccloneconfig.xml IP 資訊。|  
  
|||  
|-|-|  
|事件編號|29253|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|Virtual 網域控制站複製失敗。 複製網域控制站無法複製電腦的複製的電腦的主要網域中找到主要網域控制站 (PDC) 操作主機。<br /><br />傳回的錯誤碼是 %1 (%2)。<br /><br />請確認主要網域控制站首頁網域中的複製電腦已指派給動態網域控制站、 是 online 和正常運作。 請確認複製的電腦 LDAP 日 RPC 連接主要網域控制站需要連接埠和通訊協定。|  
|筆記與解析度|驗證複製的網域控制站 IP 和資訊 DNS 設定。 使用 Dcdiag.exe /test:locatorcheck 驗證 online PDCE 時，請使用 Nltest.exe /server:* <PDCE> * /dclist:* <domain> *有效 RPC，以取得網路擷取從 PDCE 複製失敗時和分析資料傳輸。|  
  
|||  
|-|-|  
|事件編號|29254|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|繫結至主要網域控制站 %1 virtual 網域控制站複製失敗。<br /><br />傳回的錯誤碼是 %2 (%3)。<br /><br />請確認主要網域控制站 %1 是 online 正常運作。 請確認複製的電腦 LDAP 日 RPC 連接主要網域控制站需要連接埠和通訊協定。|  
|筆記與解析度|驗證複製的網域控制站 IP 和資訊 DNS 設定。 使用 Dcdiag.exe /test:locatorcheck 驗證 online PDCE 時，請使用 Nltest.exe /server:* <PDCE> * /dclist:* <domain> *有效 RPC，以取得網路擷取從 PDCE 複製失敗時和分析資料傳輸。|  
  
|||  
|-|-|  
|事件編號|29255|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|Virtual 網域控制站複製失敗。<br /><br />嘗試建立主要網域控制站 %1 所需的影像被複製物件傳回錯誤 %2 (%3)。<br /><br />請確認複製的網域控制站複製本身的權限。 檢查有 Directory 服務事件登入主要網域控制站 %1 中相關事件。|  
|筆記與解析度|查詢特定 MS TechNet、 MS 微軟，與 MS 部落格，以判斷其一般的意義，以及然後疑難排解錯誤會根據這些結果。|  
  
|||  
|-|-|  
|事件編號|29256|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|將開機設定 Directory 服務還原模式旗標到失敗，錯誤代碼 %1。<br /><br />請查看 %systemroot%\debug\dcpromo.log 有關更多的錯誤。|  
|筆記與解析度|檢查服務 Directory 登入和 dcpromo.log 如需詳細資訊。 檢查應用程式和系統事件登。 調查第三方應用程式可能會封鎖權限使用量。|  
  
|||  
|-|-|  
|事件編號|29257|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|已完成 virtual 網域控制站複製。 將電腦重新開機失敗，錯誤代碼 %1。<br /><br />請重新開機才能完成作業複製。|  
|筆記與解析度|檢查應用程式和系統事件登。 調查第三方應用程式可能會封鎖權限使用量。|  
  
|||  
|-|-|  
|事件編號|29264|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|清除開機至 Directory 服務還原模式旗標失敗，錯誤代碼 %1。<br /><br />請查看 %systemroot%\debug\dcpromo.log 有關更多的錯誤。|  
|筆記與解析度|檢查服務 Directory 登入和 dcpromo.log 如需詳細資訊。 檢查應用程式和系統事件登。 調查第三方應用程式可能會封鎖權限使用量。|  
  
|||  
|-|-|  
|事件編號|29265|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|資訊|  
|訊息|Virtual 網域控制站複製成功。 重新命名 virtual 網域控制站複製設定檔 %1%2。|  
|筆記與解析度|不適用，這是成功事件。|  
  
|||  
|-|-|  
|事件編號|29266|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|Virtual 網域控制站複製成功。 嘗試重新命名 virtual 網域控制站複製設定檔 %1 失敗，錯誤代碼 %2 (%3)。|  
|筆記與解析度|手動重新命名 dccloneconfig.xml 檔案。|  
  
|||  
|-|-|  
|事件編號|29267|  
|來源|Microsoft Windows-對-DSROLE-伺服器|  
|嚴重性|錯誤|  
|訊息|Virtual 網域控制站複製無法檢查 virtual 網域控制站複製允許的應用程式] 清單。<br /><br />傳回的錯誤碼是 %1 (%2)。<br /><br />可能會發生這個錯誤，語法複製時發生錯誤允許清單檔案 (目前檢查檔案: %3)。 如需有關這個錯誤，請查看 %systemroot%\debug\dcpromo.log。|  
|筆記與解析度|請依照下列指示事件|  
  
##### <a name="error-messages"></a>錯誤訊息  
有失敗模擬的網域控制站複製; 不直接互動式錯誤所有複製資訊登入系統 Directory 服務登並網域控制站升級登入 dcpromo.log。 不過，如果伺服器開機進入 DS 還原模式，調查立即在升級或複製失敗。  
  
Dcpromo.log 是第一個地方以檢查複製失敗。 根據列出失敗，可能必須後續檢視 Directory 服務及系統登進一步的診斷資訊。  
  
#### <a name="known-issues-and-support-scenarios"></a>已知的問題，以及案例的支援  
以下是在 Windows Server 2012 開發程序期間常見的問題。 這些問題」的設計」，並具有有效的因應措施或更適合技巧，以避免使用它們首先。 某些可能解析較新版本的 Windows Server 2012 中。  
  
|||  
|-|-|  
|**問題**|**複製 DSRM 失敗**|  
|**症狀**|複製開機進入 Directory 服務還原模式|  
|**解析度和筆記**|驗證所有區段部署擬化檔案網域控制站區段遵循的步驟，[適用於疑難排解網域控制站複製一般方法](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_GeneralMethodology)<br /><br />以 kb 為單位 2742844 所述。|  
  
|||  
|-|-|  
|**問題**|**當您使用 DHCP 複製額外的 IP 租用**|  
|**症狀**|在成功複製 DC 使用 DHCP 之後, 複製的第一次開機需要 DHCP 租賃。 然後時伺服器會重新命名為 DC 重新啟動，所需的第二個 DHCP 租賃。 不發行的第一個 IP 位址，以及您最後使用 「 幻像 「 租用|  
|**解析度和筆記**|手動 delete dhcp 未使用的地址租用或允許通常會到期。 以 kb 為單位 2742836 所述。|  
  
|||  
|-|-|  
|**問題**|**很長的延遲之後複製到 DSRM 失敗**|  
|**症狀**|複製會顯示在 「 X 網域控制站複製是 %的完成度 」 的之間 8 到 15 分鐘的時間。 之後，複製失敗，並會開機至 DSRM。|  
|**解析度和筆記**|複製的電腦無法取得動態 IP 位址 DHCP 或 SLAAC，或使用重複的 IP 位址，或 PDC 找不到。 複製執行多個重試次數會導致延遲。 解析允許複製的網路問題。<br /><br />以 kb 為單位 2742844 所述。|  
  
|||  
|-|-|  
|**問題**|**複製未重新建立的所有服務主體名稱**|  
|**症狀**|如果一組*三個部分*服務主體名稱 (SPN) 包含同時 NetBIOS 名稱連接埠和連接埠不相同 NetBIOS 名稱、 非連接埠項目不重新建立的新的電腦名稱。 例如：<br /><br />customspn 日 DC1:200 / 符號 app1 無效使用*這個重新建立的新的電腦名稱*<br /><br />customspn 日 DC1 日 app1 符號無效使用*未重新建立的新的電腦名稱*<br /><br />完整的名稱會重新建立並重新建立 Spn 三個部分不會無論連接埠。 例如，這些會重新建立成功複製上：<br /><br />customspn / 符號 DC1:202 無效使用*這個重新建立*<br /><br />customspn 日 DC1 無效使用的符號*這個重新建立*<br /><br />customspn/DC1.corp.contoso.com:202 符號無效使用*這個重新建立的名稱*<br /><br />customspn/DC1.corp.contoso.com 符號無效使用*這個重新建立*|  
|**解析度和筆記**|這是一項限制的網域控制站重新命名程序在 Windows 中，而不只是在複製。 三個部分 SPN 不是由任何案例中重新命名邏輯操作處理。 為他們建立任何缺少 Spn 視不受影響，如此一來，最隨附的 Windows 服務。 其他應用程式可能需要手動輸入修正問題的相關 SPN。<br /><br />以 kb 為單位 2742874 所述。|  
  
|||  
|-|-|  
|**問題**|**複製失敗時，會開機至 DSRM，一般的網路錯誤**|  
|**症狀**|複製開機進入 Directory 服務修復模式。 有一般網路錯誤。|  
|解析度和筆記|確定新複製未重複靜態指派來源網域控制站; 的 MAC 位址您可以看到 VM 是否使用來執行這個命令的來源和複製虛擬機器 hypervisor 主機上靜態的 MAC 位址：<br /><br />VMName 取得-VM 的*測試-vm*和 #124;取得-VMNetworkAdapter 與 #124;fl *<br /><br />變更唯一靜態位址的 MAC 地址，或切換到使用動態 MAC 位址。<br /><br />以 kb 為單位 2742844 所述的方式|  
  
|||  
|-|-|  
|**問題**|**複製失敗時，為重複的來源 DC 會開機至 DSRM**|  
|**症狀**|新的複製開機時不需要複製。 Dccloneconfig.xml 不會重新命名，並開始 DS 還原模式中的伺服器。 會顯示錯誤 2164 Directory 服務事件登入<br /><br />*<COMPUTERNAME>*若要開始 DsRoleSvc 服務複製本機 virtual 網域控制站失敗。|  
|**解析度和筆記**|檢查服務 DS 角色伺服器服務 (DsRoleSvc) 的設定，並確定其開始輸入設定為手動。 驗證無第三方程式造成 [開始] 的這項服務。<br /><br />了解如何回收這個次要網域控制站同時確保取得複寫輸出更新的詳細資訊，會看到 Microsoft 知識庫文章 2742970。|  
  
|||  
|-|-|  
|**問題**|**複製失敗時，會開機至 DSRM，8610 錯誤**|  
|**症狀**|複製開機進入 Directory 服務還原模式。 帶領.log 顯示 8610 錯誤 （也就是 ERROR_DS_ROLE_NOT_VERIFIED 8610 或 0x21A2）|  
|**解析度和筆記**|如果可以找到 PDC，但無法執行的允許本身擔任不足複寫會發生。 例如，如果複製開始和其他系統管理員會 PDCE FSMO 角色移動到新的 DC。<br /><br />以 kb 為單位 2742916 所述。|  
  
|||  
|-|-|  
|**問題**|**複製失敗時，會開機至 DSRM，一般的網路錯誤**|  
|**症狀**|複製開機進入 Directory 服務還原模式。 有一般網路錯誤。|  
|**解析度和筆記**|確定新複製未重複靜態指派來源網域控制站; 的 MAC 位址您可以看到 VM 是否使用來執行這個命令的來源和複製虛擬機器 HYPER-V 主機上靜態的 MAC 位址：<br /><br />VMName 取得-VM 的*測試-vm*和 #124;取得-VMNetworkAdapter 與 #124;fl *<br /><br />變更唯一靜態位址的 MAC 地址，或切換到使用動態 MAC 位址。<br /><br />以 kb 為單位 2742844 所述。|  
  
|||  
|-|-|  
|**問題**|**複製失敗時，會開機至 DSRM**|  
|**症狀**|複製開機進入 Directory 服務修復模式|  
|**解析度和筆記**|請確定 dccloneconfig.xml 包含架構定義 （看到 sampledccloneconfig.xml、 行 2）：<br /><br />**< d3c:DCCloneConfig xmlns:d3c="uri:microsoft.com:schemas:DCCloneConfig] >**<br /><br />以 kb 為單位 2742844 所述的方式|  
  
|||  
|-|-|  
|問題|**無法登入伺服器會使用錯誤登入 DSRM**|  
|**症狀**|複製開機進入 Directory 服務修復模式。 您嘗試登入收到錯誤訊息：<br /><br />**目前有不登入伺服器可服務登入要求**|  
|**解析度和筆記**|請確定您的系統管理員 DSRM 帳號，並不核對登入。 使用向左箭號，然後輸入的使用者名稱：<br /><br />**。 \administrator**<br /><br />以 kb 為單位 2742908 所述的方式|  
  
|||  
|-|-|  
|**問題**|**複製來源失敗到 DSRM，錯誤**|  
|**症狀**|複製，期間失敗 8437 「 建立俠複製物件 PDC 無法在 」 (0x20f5)|  
|**解析度和筆記**|重複的電腦名稱是在 DCCloneConfig.xml 做為來源俠或設定現有俠。 電腦名稱也必須 NetBIOS 電腦名稱的格式 （15 字元或較少，不 FQDN）。<br /><br />修正檔案 dccloneconfig.xml 設定唯一、 有效的名稱。<br /><br />以 kb 為單位 2742959 所述的方式|  
  
|||  
|-|-|  
|**問題**|**新 addccloneconfigfile 錯誤 「 索引超出範圍 」**|  
|**症狀**|當新 addccloneconfigfile cmdlet 執行的是，您收到錯誤訊息：<br /><br />指數超出範圍。 必須非負和小於的收藏大小。|  
|**解析度和筆記**|系統管理員提升權限 Windows PowerShell 主控台中，您必須執行 cmdlet。 錯誤的原因是在電腦上系統管理員本機群組成員資格缺乏。<br /><br />以 kb 為單位 2742927 所述的方式|  
  
|||  
|-|-|  
|**問題**|**複製重複俠失敗**|  
|**症狀**|複製開機，而不需要複製，現有的來源 DC 重複項目|  
|**解析度和筆記**|電腦複製開始使用，但不包含任何支援的位置，DcCloneConfig.xml 檔案並不了來源網域控制站的重複的 IP 位址。 為了避免資料遺失 DC 必須正確移除。<br /><br />以 kb 為單位 2742970 所述的方式|  
  
|||  
|-|-|  
|**問題**|**新 ADDCCloneConfigFile 失敗，並伺服器時，無法運作的錯誤它會檢查來源網域控制站不 GC 是否複製網域控制站群組成員。**|  
|**症狀**|新增-ADDCCloneConfigFile 建立 dccloneconfig.xml 檔案執行的是，當您收到錯誤訊息：<br /><br />程式碼-伺服器不操作|  
|**解析度和筆記**|從您執行新-ADDCCloneConfigFile 並確認該的來源網域控制站在複製網域控制站群組成員資格已經複製到該 GC 伺服器驗證 GC 連接。<br /><br />清除位置 GC 或俠可能已被離線最近案例俠定位器快取的方式執行下列命令：<br /><br />程式碼-nltest /dsgetdc: /GC /FORCE|  
  
### <a name="advanced-troubleshooting"></a>進階疑難排解  
這個模組設法教導進階疑難排解，透過*使用*登某些解釋所發生的範例，以。 如果您了解成功模擬的網域控制站作業的外觀，失敗明顯地在您的環境中。 這些登的出示來源的遞增訂單以*如預期般*（即使是警告和錯誤） 的活動相關複製的網域控制站在每個登入。  
  
#### <a name="cloning-a-domain-controller"></a>複製網域控制站  
複製網域控制站使用 DHCP，以取得 IP 位址、 複製 SYSVOL 在此範例中，使用 FRS 或 DFSR （所需的查看適當的登入，） 首先，並使用空白 dccloneconfig.xml 檔案。  
  
##### <a name="directory-services-event-log"></a>Directory 服務事件登入  
Directory 服務登入包含大部分事件基礎複製操作資訊。 Hypervisor 變更 VM 新一代 ID 及 NTDS 服務筆記它，然後失效 RID 集區變更叫用 id。 設定新的 VM 新一代 ID 和伺服器會複寫 Active Directory 輸入資料。 DFSR 服務已停止和刪除裝載 SYSVOL 其資料庫，強迫未經授權的同步輸入。 調整 USN 高浮水印。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**2160**|ActiveDirectory_DomainService|本機 Active Directory Domain Services 發現 virtual 網域控制站複製設定檔。<br /><br />在找到 virtual 網域控制站複製設定檔：<br /><br />*<path>*\DCCloneConfig.xml<br /><br />差 virtual 網域控制站複製設定檔表示本機 virtual 網域控制站其他 virtual 網域控制站的複本。 Active Directory Domain Services 將會開始複製本身。|  
|**2191**|ActiveDirectory_DomainService|Active Directory Domain Services 設定下列登錄值來停用 DNS 更新。<br /><br />登錄鍵：<br /><br />SYSTEM\CurrentControlSet\Services\Netlogon\Parameters<br /><br />登錄值：<br /><br />UseDynamicDns<br /><br />登錄數值資料：<br /><br />0<br /><br />複製程序，在本機電腦可能會有一小段時間複製來源電腦相同電腦的名稱。 DNS A AAAA 記錄登記會停用在這段期間，因此戶端找不到本機電腦目前正在進行複製傳送要求。 複製程序將會複製完成後，再試一次讓 DNS 更新。|  
|**2191**|ActiveDirectory_DomainService|Active Directory Domain Services 設定下列登錄值來停用 DNS 更新。<br /><br />登錄鍵：<br /><br />SYSTEM\CurrentControlSet\Services\Dnscache\Parameters<br /><br />登錄值：<br /><br />RegistrationEnabled<br /><br />登錄數值資料：<br /><br />0<br /><br />複製程序，在本機電腦可能會有一小段時間複製來源電腦相同電腦的名稱。 DNS A AAAA 記錄登記會停用在這段期間，因此戶端找不到本機電腦目前正在進行複製傳送要求。 複製程序將會複製完成後，再試一次讓 DNS 更新。<br /><br />「 資訊 2 月 7 日 2012年 3:12:49 PM Microsoft-Windows-ActiveDirectory_DomainService 2191 內部設定 「 Active Directory Domain Services 下列登錄將值設為停用 DNS 更新。<br /><br />登錄鍵：<br /><br />SYSTEM\CurrentControlSet\Services\Tcpip\Parameters<br /><br />登錄值：<br /><br />DisableDynamicUpdate<br /><br />登錄數值資料：<br /><br />1<br /><br />複製程序，在本機電腦可能會有一小段時間複製來源電腦相同電腦的名稱。 DNS A AAAA 記錄登記會停用在這段期間，因此戶端找不到本機電腦目前正在進行複製傳送要求。 複製程序將會複製完成後，再試一次讓 DNS 更新。|  
|**2172**|ActiveDirectory_DomainService|朗讀 msDS GenerationId 網域控制站的電腦物件的屬性。<br /><br />msDS-GenerationId 屬性的值：<br /><br />*<Number>*|  
|**2170**|ActiveDirectory_DomainService|偵測到代 ID 變更。<br /><br />代來電顯示快取中 DS （舊值）：<br /><br />*<Number>*<br /><br />代目前中 ID VM （新值）：<br /><br />*<Number>*<br /><br />一樣快照的應用程式、 一樣匯入操作後或即時移轉作業之後，就會發生代 ID 變更。 Active Directory Domain Services 會建立新的叫用 ID 復原網域控制站。 應還原模擬的網域控制站使用一樣的快照。 支援的方法來還原或復原 content 的 Active Directory Domain Services 資料庫是還原所做的 Active Directory Domain Services 注意備份應用程式的系統狀態備份。|  
|**1109**|ActiveDirectory_DomainService|已變更此 directory 伺服器呼叫識別碼屬性。 在建立備份之的時間的最高更新序號如下：<br /><br />呼叫識別碼屬性 （舊值）：<br /><br />*<GUID>*<br /><br />呼叫識別碼屬性 （新值）：<br /><br />*<GUID>*<br /><br />更新的序號：<br /><br />*<Number>*<br /><br />呼叫識別碼 directory 伺服器還原備份媒體，從時變更設定為主控寫入應用程式 directory 磁碟分割、 已繼續一樣快照套用之後，在一樣匯入作業，或之後即時移轉作業。 應還原模擬的網域控制站使用一樣的快照。 還原或回復到支援的方法的 Active Directory Domain Services 資料庫 content 是還原進行備份 Active Directory Domain Services 感知應用程式的系統狀態備份。|  
|**1000**|ActiveDirectory_DomainService|Microsoft Active Directory Domain Services 開機完成。|  
|**1394**|ActiveDirectory_DomainService|已經清除所有 Active Directory Domain Services 資料庫防止更新的問題。 Active Directory Domain Services 資料庫的新更新的成功。 此時網路登入服務。|  
|**2163**|ActiveDirectory_DomainService|DsRoleSvc 服務已複製本機 virtual 網域控制站開始。|  
|**326**|NTDS ISAM|NTDS (536) NTDSA： 資料庫引擎連接資料庫 (1、 C:\Windows\NTDS\ntds.dit)。 (時間 = 0 秒)<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.000 [3]、 [4] 0.000、 0.000 [5]、 [6] 0.016、 0.000 [7]、 0.000 [8]、 [9] 0.000、 0.000 [10]、 0.000 [11]、 [12] 0.000。<br /><br />儲存快取︰ 1|  
|**103**|NTDS ISAM|NTDS (536) NTDSA： 資料庫引擎停止 (0) 的執行個體。<br /><br />不正常關機： 0<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.000 [3]、 [4] 0.000、 0.032 [5]、 [6] 0.000、 0.000 [7]、 0.000 [8]、 0.031 [9]、 0.000 [10]、 0.000 [11]、 [12] 0.000、 0.000 [13]、 [14] 0.000、 0.000 [15]。|  
|**102**|NTDS ISAM|NTDS (536) NTDSA： 資料庫引擎 (6.02.8225.0000) 新執行個體 (0) 開始。|  
|**105**|NTDS ISAM|NTDS (536) NTDSA： 資料庫引擎會開始新的執行個體 (0)。 (時間 = 0 秒)<br /><br />內部的時間順序： 0.016 [1] 0.000 [2]、 0.015 [3]、 [4] 0.078、 [5] 0.000、 0.000 [6]、 [7] 0.000、 0.000 [8]、 [9] 0.046、 0.000 [10]、 0.000 [11]。|  
|**1004**|ActiveDirectory_DomainService|Active Directory Domain Services 成功關機。|  
|**102**|NTDS ISAM|NTDS (536) NTDSA： 資料庫引擎 (6.02.8225.0000) 新執行個體 (0) 開始。|  
|**326**|NTDS ISAM|NTDS (536) NTDSA： 資料庫引擎連接資料庫 (1、 C:\Windows\NTDS\ntds.dit)。 (時間 = 0 秒)<br /><br />內部的時間順序： 0.000 [1] 0.015 [2]、 0.016 [3]、 [4] 0.000、 0.031 [5]、 [6] 0.000、 0.000 [7]、 0.000 [8]、 [9] 0.000、 0.000 [10]、 [11] 0.000、 0.000 [12]。<br /><br />儲存快取︰ 1|  
|**105**|NTDS ISAM|NTDS (536) NTDSA： 資料庫引擎會開始新的執行個體 (0)。 (時間 = 1 秒)<br /><br />內部的時間順序： 0.031 [1] 0.000 [2]、 0.000 [3]、 [4] 0.391、 [5] 0.000、 0.000 [6]、 [7] 0.000、 0.000 [8]、 [9] 0.031、 0.000 [10]、 0.000 [11]。|  
|**1109**|ActiveDirectory_DomainService|已變更此 directory 伺服器呼叫識別碼屬性。 在建立備份之的時間的最高更新序號如下：<br /><br />呼叫識別碼屬性 （舊值）：<br /><br />*<GUID>*<br /><br />呼叫識別碼屬性 （新值）：<br /><br />*<GUID>*<br /><br />更新的序號：<br /><br />*<Number>*<br /><br />呼叫識別碼 directory 伺服器還原備份媒體，從時變更設定為主控寫入應用程式 directory 磁碟分割、 已繼續一樣快照套用之後，在一樣匯入作業，或之後即時移轉作業。 應還原模擬的網域控制站使用一樣的快照。 還原或回復到支援的方法的 Active Directory Domain Services 資料庫 content 是還原進行備份 Active Directory Domain Services 感知應用程式的系統狀態備份。|  
|**1168**|ActiveDirectory_DomainService|內部錯誤： Active Directory Domain Services 錯誤。<br /><br />其他資料<br /><br />錯誤值 （小數點）：<br /><br />2<br /><br />錯誤值 （十六進位）：<br /><br />2<br /><br />內部 ID:<br /><br />7011658|  
|**1110**|ActiveDirectory_DomainService|首先這個網域控制站升級將會延遲下列間隔。<br /><br />間隔 （分鐘）：<br /><br />5<br /><br />這樣可以準備需要的 directory 磁碟分割通用通知時，所需此延遲。 在登錄中，您可以指定 directory 系統代理程式之前重點首先本機網域控制站等待秒數。 如需全球 Catalog 延遲廣告登錄值，查看資源套件散發系統指南|  
|**103**|NTDS ISAM|NTDS (536) NTDSA： 資料庫引擎停止 (0) 的執行個體。<br /><br />不正常關機： 0<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.000 [3]、 [4] 0.000、 0.047 [5]、 [6] 0.000、 0.000 [7]、 0.000 [8]、 0.016 [9]、 0.000 [10]、 0.000 [11]、 [12] 0.000、 0.000 [13]、 [14] 0.000、 0.000 [15]。|  
|**1004**|ActiveDirectory_DomainService|Active Directory Domain Services 成功關機。|  
|**1539**|ActiveDirectory_DomainService|Active Directory Domain Services 無法停用軟體的磁碟寫入快取下列硬碟上。<br /><br />硬碟：<br /><br />c:<br /><br />可能會遺失資料系統失敗|  
|**2179**|ActiveDirectory_DomainService|MsDS-GenerationId 物件的屬性網域控制站的電腦已設定為下列參數：<br /><br />GenerationID 屬性：<br /><br />*<Number>*|  
|**2173**|ActiveDirectory_DomainService|無法讀取 msDS-GenerationId 網域控制站的電腦物件的屬性。 這可能因為資料庫交易失敗，或在本機資料庫代 id 不存在。 之後帶領或俠不 virtual 網域控制站 msDS GenerationId 不存在期間的第一次重新開機。<br /><br />其他資料<br /><br />失敗碼：<br /><br />6|  
|**1000**|ActiveDirectory_DomainService|Microsoft Active Directory Domain Services 開機完成，6.2.8225.0 版本|  
|**1394**|ActiveDirectory_DomainService|已經清除所有 Active Directory Domain Services 資料庫防止更新的問題。 Active Directory Domain Services 資料庫的新更新的成功。 此時網路登入服務。|  
|**1128**|ActiveDirectory_DomainService|1128 認知一致性檢查 」 複寫連接已從下列來源 directory 服務建立本機 directory 服務。<br /><br />來源 directory 服務：<br /><br />DATA-CN = NTDS 設定]*<Domain Controller DN>*<br /><br />本機 directory 服務：<br /><br />DATA-CN = NTDS 設定]*<Domain Controller DN>*<br /><br />其他資料<br /><br />原因代碼：<br /><br />0x2<br /><br />建立點內部 ID:<br /><br />f0a025d|  
|**1999**|ActiveDirectory_DomainService|來源 directory 服務已最佳化的更新序號 (USN) 出示目的地 directory 服務。 來源和目的地 directory 服務有常見複寫合作夥伴。 目的地 directory 服務是最新的常見問題︰ 複寫合作夥伴，並安裝來源 directory 服務使用此合作夥伴的備份。<br /><br />目的地 directory 服務 ID:<br /><br />*<GUID> (<FQDN>)*<br /><br />一般 directory 服務 ID:<br /><br />*<GUID>*<br /><br />常見的屬性 USN:<br /><br />*<Number>*<br /><br />如此一來，最新的向量的目的地 directory 服務已使用下列設定。<br /><br />先前的物件 USN:<br /><br />0<br /><br />先前屬性 USN:<br /><br />0<br /><br />資料庫 GUID:<br /><br />*<GUID>*<br /><br />物件 USN:<br /><br />*<Number>*<br /><br />屬性 USN:<br /><br />*<Number>*|  
  
##### <a name="system-event-log"></a>系統事件登入  
下一步的複製操作指示位於系統事件登入。 Hypervisor 告訴來賓電腦已複製或還原的開發進程的快照，為網域控制站立即使其 RID 集區，以避免稍後複製安全性原則。 複製繼續，各種預期的作業和訊息會顯示，大部分操作開始和停止的服務，某些預期這造成錯誤。 當您完成整體複製成功系統事件登入資訊。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**16654**|薩姆-directory-服務|已無效 account 識別碼 (Rid) 的集區。 這可能是因為預期如下：<br /><br />1.網域控制站是從備份還原。<br /><br />2.網域控制站執行一樣會還原的快照。<br /><br />3.系統管理員已手動失效集區|  
|**7036**|服務控制管理員|Active Directory Domain Services 服務進入執行的狀態。|  
|**7036**|服務控制管理員|Kerberos 鍵 Distribution 中心服務輸入執行的狀態。|  
|**3096**|Netlogon|找不到此網域主要網域控制站。|  
|**7036**|服務控制管理員|安全性帳號管理員服務輸入執行的狀態。|  
|**7036**|服務控制管理員|伺服器服務輸入執行的狀態。|  
|**7036**|服務控制管理員|Netlogon 服務輸入執行的狀態。|  
|**7036**|服務控制管理員|Active Directory Web 服務服務進入執行的狀態。|  
|**7036**|服務控制管理員|DFS 複寫服務輸入執行的狀態。|  
|**7036**|服務控制管理員|檔案複製服務輸入執行的狀態。|  
|**14533**|Microsoft-Windows-DfsSvc|建置所有命名空間已經 DFS。|  
|**14531**|Microsoft-Windows-DfsSvc|完成初始化 DFS 伺服器。|  
|**7036**|服務控制管理員|DFS 命名空間服務進入執行的狀態。|  
|**7023**|服務控制管理員|間的訊息中心服務終止錯誤如下：<br /><br />指定的伺服器無法執行要求的作業。|  
|**7036**|服務控制管理員|間的訊息中心服務輸入停止的狀態。|  
|**5806**|Netlogon|動態更新已在這個網域控制站手動停用。<br /><br />使用者動作<br /><br />重新設定使用動態更新，或是手動新增 DNS 記錄從檔案 '%systemroot%\system32\config\netlogon.dns' DNS 資料庫這個網域控制站 」。|  
|**16651**|薩姆-directory-服務|新的 account 識別碼集區要求失敗。 成功要求之前，將會重試作業。 錯誤<br /><br />FSMO 操作失敗。 無法連絡目前 FSMO 擁有者。|  
|**7036**|服務控制管理員|輸入的 DNS 伺服器服務執行的狀態。|  
|**7036**|服務控制管理員|DS 角色伺服器服務輸入執行的狀態。|  
|**7036**|服務控制管理員|Netlogon 服務輸入停止的狀態。|  
|**7036**|服務控制管理員|檔案複製服務輸入停止的狀態。|  
|**7036**|服務控制管理員|Kerberos 鍵 Distribution 中心服務輸入停止的狀態。|  
|**7036**|服務控制管理員|DNS 伺服器服務輸入停止的狀態。|  
|**7036**|服務控制管理員|Active Directory Domain Services 服務輸入停止的狀態。|  
|**7036**|服務控制管理員|Netlogon 服務輸入執行的狀態。|  
|**7040**|服務控制管理員|停用來從自動開始變更 [開始] 畫面的 Active Directory Domain Services 服務類型。|  
|**7036**|服務控制管理員|Netlogon 服務輸入停止的狀態。|  
|**7036**|服務控制管理員|檔案複製服務輸入執行的狀態。|  
|**29219**|對 DSROLE 伺服器|Virtual 網域控制站複製成功。|  
|**29223**|對 DSROLE 伺服器|這個伺服器現在已網域控制站。|  
|**29265**|對 DSROLE 伺服器|Virtual 網域控制站複製成功。 複製設定檔 C:\Windows\NTDS\DCCloneConfig.xml virtual 網域控制站已 C:\Windows\NTDS\DCCloneConfig.20120207-151533.xml 以重新命名。|  
|**1074**|User32|此程序 C:\Windows\system32\lsass.exe (DC2) 已車載機起始 DC2 的電腦重新開機，代表使用者 NT AUTHORITY\SYSTEM 原因如下： 作業系統： 重新設定 （計劃中）<br /><br />程式碼的原因︰ 0x80020004<br /><br />關機類型： 重新開機<br /><br />意見: 」|  
  
##### <a name="dcpromolog"></a>帶領。登入  
Dcpromo.log 包含實際促銷複製的一部分無法描述 Directory 服務事件登入。 登入不提供的事件登入的項目傳授的解釋層級，因為模組的此一節包含其他附注。  
  
升級程序表示複製開始，DC 在其目前的組態刪除，並重新升級使用現有的廣告資料庫 （更像是 IFM 升級），然後 DC 複製輸入的變更 delta 的廣告且 SYSVOL，和複製已完成。  
  
> [!NOTE]  
> 登入可讀性，本單元已修改日期欄中移除。  
  
> [!NOTE]  
> 進一步解釋 dcpromo.log 看到瞭解與疑難排解 AD DS 簡化管理 Windows Server 2012 中。  
>   
> [https://go.microsoft.com/fwlink/p/?LinkId=237244](https://go.microsoft.com/fwlink/p/?LinkId=237244)  
  
-   複製型促銷 [開始]  
  
-   設定 Directory 服務還原模式旗標伺服器會不會開機備份通常為原始的複製和原因命名或 Directory 服務衝突，  
  
-   更新服務 Directory 事件登入  
  
```  
15:14:01 [INFO] vDC Cloneing: Setting Boot into DSRM flag succeeded.  
15:14:01 [WARNING] Cannot get user Token for Format Message: 1725l  
15:14:01 [INFO] vDC Cloning: Created vDCCloningUpdate event.  
15:14:01 [INFO] vDC Cloning: Created vDCCloningComplete event.  
```  
  
-   因此，不會通知的網域控制站停止 NetLogon 服務  
  
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
  
-   檢查 dccloneconfig.xml 管理員指定自訂設定檔。  
  
-   在此範例案例是空白的檔案，因此專自動到 [所有設定，而且自動 IP 位址所需的網路  
  
```  
15:14:02 [INFO] vDC Cloning: Clone config file C:\Windows\NTDS\DCCloneConfig.xml is considered to be a blank file (containing 0 bytes)  
15:14:02 [INFO] vDC Cloning: Parsing clone config file C:\Windows\NTDS\DCCloneConfig.xml returned HRESULT 0x0  
```  
  
-   確認您不有任何服務或不是 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 的安裝程式  
  
```  
15:14:02 [INFO] vDC Cloning: Checking allowed list:  
15:14:03 [INFO] vDC Cloning: Completed checking allowed list:  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   由於系統管理員不指定 IP 資訊，讓 DHCP 網路介面卡  
  
```  
15:14:03 [INFO] vDC Cloning: Enable DHCP:  
15:14:03 [INFO] WMI Instance: Win32_NetworkAdapterConfiguration.Index=12  
15:14:03 [INFO] Method: EnableDHCP  
15:14:03 [INFO] HRESULT code: 0x0 (0)  
15:14:03 [INFO] Return Value: 0x0 (0)  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:03 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
```  
  
-   找出肯定  
  
-   設定 （在本案例自動產生） 複製的網站  
  
-   設定複製的名稱 （在本案例自動產生）  
  
```  
15:14:03 [INFO] vDC Cloning: Found PDC. Name: DC1.root.fabrikam.com  
15:14:04 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:04 [INFO] vDC Cloning: Winlogon UI Notification #1: Domain Controller cloning is at 5% completion...  
15:14:05 [INFO] vDC Cloning: Winlogon UI Notification #2: Domain Controller cloning is at 10% completion...  
15:14:05 [INFO] vDC Cloning: Set vDCCloningUpdate event.  
15:14:05 [INFO] Site of the cloned DC: Default-First-Site-Name  
```  
  
-   建立新的複製電腦物件  
  
-   重新命名的複製符合新名稱  
  
```  
15:14:05 [INFO] vDC Cloning: Clone DC objects are created on PDC.  
15:14:05 [INFO] Name of the cloned DC: DC2-CL0001  
15:14:05 [INFO] DsRolepSetRegStringValue on System\CurrentControlSet\Services\NTDS\Parameters\CloneMachineName to DC2-CL0001 returned 0  
15:14:05 [INFO] vDC Cloning: Save CloneMachineName in registry: 0x0 (0)  
```  
  
-   提供促銷設定，根據先前 dccloneconfig.xml 或自動代規則  
  
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
  
-   [開始升級]  
  
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
  
-   停止和設定的所有 AD DS 相關服務 NTDS、 NTFRS 日 DFSR、 \ [KDC （DNS）  
  
> [!NOTE]  
> 關閉拍攝一段時間 DNS 服務會如預期般在本案例中，它會使用已不再提供更之前停止-NTDS 服務的廣告整合區域看到模組的此一節所述稍後 DNS 事件。  
  
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
  
-   強制 NT5DS (NTP) 時間同步處理的其他網域控制站 (通常是 PDCE)  
  
```  
15:15:02 [INFO] Forcing time sync  
```  
  
-   請連絡擁有來源網域控制站 account 複製的網域控制站  
  
-   清除任何的現有 Kerberos 門票  
  
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
  
-   停止 NetLogon 服務，並將其開始輸入  
  
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
  
-   設定為自動執行 DFSR 日 NTFRS 服務  
  
-   Delete 強制 SYSVOL 的非授權同步服務接下來開始時他們現有資料庫檔案  
  
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
  
-   開始使用現有的 NTDS 資料庫檔案在升級程序  
  
-   連絡 RID 主機  
  
> [!NOTE]  
> AD DS 未實際上安裝服務以下，這是舊版檢測登入  
  
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
  
-   變更現有的來源電腦資料庫中有叫用 ID  
  
-   建立新物件 NTDS 設定的這個複製  
  
-   複製 AD 物件 delta 從合作夥伴網域控制站在  
  
> [!NOTE]  
> 即使所有物件複寫都列，這是 subsume 更新所需的只中繼資料。 複製 NTDS 資料庫中的所有變更的物件已存在，並不需要複製再試一次，就像使用 ifm 升級。  
  
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
  
-   填入磁碟分割 GC 所需的任何缺少的更新  
  
-   完成升級的重大部分 AD DS  
  
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
  
-   完成 SYSVOL 輸入的的複寫  
  
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
  
-   讓 client DNS 登記  
  
```  
15:15:18 [INFO] vDC Cloning: Set DisableDynamicUpdate reg value to 0 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set UseDynamicDns reg value to 1 to enable dynamic update records registration.  
15:15:18 [INFO] vDC Cloning: Set RegistrationEnabled reg value to 1 to enable dynamic update records registration.  
```  
  
-   執行指定 DefaultDCCloneAllowList.xml SYSPREP 模組<SysprepInformation>的項目。  
  
```  
15:15:18 [INFO] vDC Cloning: Running sysprep providers.  
15:15:32 [INFO] vDC Cloning: Completed running sysprep providers.  
```  
  
-   複製升級已完成  
  
-   移除 DSRM 開機旗標讓伺服器開機通常會在下一次  
  
-   重新命名 dccloneconfig.xml，讓它不再次朗讀下一個開機。  
  
-   電腦重新開機  
  
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
  
##### <a name="active-directory-web-services-event-log"></a>Active Directory Web 服務事件登入  
複製發生，NTDS 時。DIT 資料庫通常是長期離線。 ADWS 服務此登至少一個事件。 複製完成之後，ADWS 服務開始，筆記，但尚未有效的電腦憑證但 （可能或可能不會根據您的環境部署 Microsoft PKI 自動註冊使用或不），再開始新的網域控制站的執行個體。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**1202**|ADWS 執行個體事件|這台電腦現在裝載指定的 directory 執行個體，但 Active Directory 網頁的服務可能會不服務的方法。 Active Directory Web 服務定期將會重試這項操作。<br /><br />Directory 執行個體： NTDS<br /><br />Directory 執行個體 LDAP 連接埠： 389<br /><br />Directory 執行個體 SSL 連接埠： 636|  
|**1000**|ADWS 執行個體事件|開始 active Directory Web 服務|  
|**1008**|ADWS 執行個體事件|Active Directory Web 服務已成功減少其安全性特殊權限|  
|**1100**|ADWS 執行個體事件|中指定的值<appsettings>無誤已載入的設定檔的 Active Directory Web 服務一節。|  
|**1400**|ADWS 執行個體事件|ADWS 憑證事件 「 Active Directory Web 服務找不到伺服器的憑證，以指定的憑證的名稱。 憑證，才能使用 SSL 日 TLS 連接。 若要使用 SSL 日 TLS 連接，確認有效的伺服器驗證憑證的受信任憑證授權單位已安裝在電腦上。<br /><br />憑證的名稱︰*<Server FQDN>*|  
|**1100**|ADWS 執行個體事件|中指定的值<appsettings>無誤已載入的設定檔的 Active Directory Web 服務一節。|  
|**1200**|ADWS 執行個體事件|Active Directory Web 服務為現在提供服務指定的 directory 執行個體。<br /><br />Directory 執行個體： NTDS<br /><br />Directory 執行個體 LDAP 連接埠： 389<br /><br />Directory 執行個體 SSL 連接埠： 636|  
  
##### <a name="dns-server-event-log"></a>DNS 伺服器事件登入  
DNS 服務會簡短預期的中斷時遇到複製發生，請在 DNS 服務仍在執行時離線 AD DS 資料庫。 這是使用 Active Directory 整合 DNS，但不是是否使用標準主要或次要 DNS。 這些錯誤登入多個時間。 複製完成後，請 DNS 會上恢復正常。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**4013**|DNS 伺服器服務|DNS 伺服器正在等待通知的 directory 初始同步已經完成的 Active Directory Domain Services (AD DS)。 完成初始同步是因為 DNS 重要的資料可能尚未會複寫加入網域控制站的 DNS 伺服器服務才能開始。 如果事件 AD DS 事件木頭中的指示 DNS 名稱解析發生問題，請考慮將這個網域中的其他 DNS 伺服器的 IP 位址新增到 DNS 伺服器清單中的 [這部電腦網際網路通訊協定屬性。 這個事件將會登入之前 AD DS 發出初始同步已成功完成 10 分鐘。|  
|**4015**|DNS 伺服器服務|DNS 伺服器發生嚴重錯誤的 Active Directory。 檢查 Active Directory 正常運作。 （這可能會是空白） 的延伸的錯誤偵錯資訊 」 」 」。 事件資料會包含錯誤。|  
|**4000**|DNS 伺服器服務|打開 Active Directory 無法 DNS 伺服器。  此 DNS 伺服器設定為取得並使用此區域 directory 資訊，無法載入而它的區域。  檢查 Active Directory 正常運作，並重新載入的區域。 事件資料是錯誤碼。|  
|**4013**|DNS 伺服器服務|DNS 伺服器正在等待通知的 directory 初始同步已經完成的 Active Directory Domain Services (AD DS)。 完成初始同步是因為 DNS 重要的資料可能尚未會複寫加入網域控制站的 DNS 伺服器服務才能開始。 如果事件 AD DS 事件木頭中的指示 DNS 名稱解析發生問題，請考慮將這個網域中的其他 DNS 伺服器的 IP 位址新增到 DNS 伺服器清單中的 [這部電腦網際網路通訊協定屬性。 這個事件將會登入之前 AD DS 發出初始同步已成功完成 10 分鐘。|  
|**2**|DNS 伺服器服務|開始 DNS 伺服器。|  
|**4**|DNS 伺服器服務|完成區域的背景載入 DNS 伺服器。 允許的他們個人的區域的設定，現在已有 DNS 更新及區域轉送，所有區域。|  
  
##### <a name="file-replication-service-event-log"></a>檔案複製服務事件登入  
複製檔案服務同步處理非系統授權的合作夥伴期間複製。 複製這樣的優勢刪除 NTFRS 資料庫檔案並保持不變，作為預先植資料 SYSVOL 到。 若要同步的兩個嘗試會如預期般。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**13562**|NtFrs|以下是摘要的警告與檔案複寫服務雖然輪詢 FRS 複本的網域控制站 DC2.root.fabrikam.com 設定設定資訊時發生錯誤。<br /><br />無法為網域控制站連結。 將會再試一次在下一個輪詢循環|  
|**13502**|NtFrs|停止檔案複寫服務。|  
|**13565**|NtFrs|檔案複寫服務初始化從另一部網域控制站的資料與系統磁碟區。 此程序完成之前電腦 DC2 無法加入網域控制站。 然後將會為 SYSVOL 共用系統磁碟區。<br /><br />若要檢查 SYSVOL 共用，請在命令提示字元中，輸入<br /><br />共用網路<br /><br />在檔案複寫服務完成初始設定程序，會出現 SYSVOL 共用。<br /><br />系統磁碟區的初始設定可能需要一些時間。 是的數據量中的系統磁碟區，可供使用的其他網域控制站，並網域控制站複寫間隔而定。|  
|**13501**|NtFrs|開始檔案複寫服務|  
|**13502**|NtFrs|停止檔案複寫服務。|  
|**13503**|NtFrs|檔案複製服務已停止。|  
|**13565**|NtFrs|檔案複寫服務初始化從另一部網域控制站的資料與系統磁碟區。 此程序完成之前電腦 DC2 無法加入網域控制站。 然後將會為 SYSVOL 共用系統磁碟區。<br /><br />若要檢查 SYSVOL 共用，請在命令提示字元中，輸入<br /><br />共用網路<br /><br />在檔案複寫服務完成初始設定程序，會出現 SYSVOL 共用。<br /><br />系統磁碟區的初始設定可能需要一些時間。 是的數據量中的系統磁碟區，可供使用的其他網域控制站，並網域控制站複寫間隔而定。|  
|**13501**|NtFrs|開始工作檔案複寫服務。|  
|**13553**|NtFrs|檔案複寫服務順利加入這台電腦複本下列設定：<br /><br />「 網域系統磁碟區 （SYSVOL 共用） 」<br /><br />這個事件和相關的資訊如下所示：<br /><br />電腦 DNS 名稱*<Domain Controller FQDN>*<br /><br />複本設定成員名稱*<Domain Controller>*<br /><br />複本設定根路徑*<path>*<br /><br />臨時 directory 路徑複本*<path>*<br /><br />已正常運作的路徑複本*<path>*|  
|**13520**|NtFrs|將檔案複製服務移動既有的檔案<path>到* <path> *\NtFrs_PreExisting___See_EventLog。<br /><br />複製檔案服務可能 delete 中的檔案* <path>*隨時 \NtFrs_PreExisting___See_EventLog。 檔案可以複製全新儲存刪除從* <path> *\NtFrs_PreExisting___See_EventLog。 如果已經有一些其他複製合作夥伴這些檔案將檔案複製到 c:\windows\sysvol\domain 可能導致衝突名稱。<br /><br />有時候，檔案複寫服務可能複製檔案的* <path> *\NtFrs_PreExisting___See_EventLog 到* <path> *而不是複製檔案的其他複製協力廠商。<br /><br />空間可以隨時復原刪除中的檔案的* <path> *\NtFrs_PreExisting___See_EventLog。 」|  
|**13508**|NtFrs|他檔案複寫服務時發生問題，讓從複寫* \\\\ <Domain Controller FQDN> *以* <Domain Controller> *為* <path> *使用<br /><br />DNS name *\\\\<Domain Controller FQDN>*. 將會持續 FRS 重試。<br /><br />以下是一些您將會看到這個警告的原因。<br /><br />[1] FRS 無法正確地將 DNS 名稱解析* \\\\ <Domain Controller FQDN> *來自此電腦。<br /><br />[不執行 FRS 2] * \\\\ <Domain Controller FQDN> *。<br /><br />[所有的網域控制站尚未複寫 Active Directory Domain Services 進行這個複本中的 3] 拓撲資訊。<br /><br />連接，每修正這個問題之後，您會看到其他事件登入訊息，指出已建立的連接後，就會出現這個事件登入的訊息。|  
|**13509**|NtFrs|檔案複寫服務已支援複寫從* \\\\ <Domain Controller FQDN> *以* <Domain Controller> *為* <Path> *重複重試後。|  
|**13516**|NtFrs|檔案複寫服務不讓電腦* <Domain Controller> *的網域控制站變得。 已成功初始化系統磁碟區，已經通知 Netlogon 服務，系統磁碟區現在已經準備好 SYSVOL 共用。<br /><br />輸入 「 共用網路] 來檢查是否有 SYSVOL 分享。 」|  
  
##### <a name="dfs-replication-event-log"></a>DFS 複寫事件登入  
DFSR 服務同步處理非系統授權的合作夥伴期間複製。 複製這樣的優勢刪除 DFSR 資料庫檔案並保持不變，作為預先植資料 SYSVOL 到。 若要同步的兩個嘗試會如預期般。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**1004**|DFSR|開始 DFS 複寫服務。|  
|**1314**|DFSR|DFS 複寫服務成功設定的偵錯登入的檔案。<br /><br />其他資訊：<br /><br />偵錯登入的檔案路徑： C:\Windows\debug|  
|**6102**|DFSR|DFS 複寫服務已成功已經登記完畢 WMI 提供者|  
|**1206**|DFSR|DFS 複寫服務順利連絡網域控制站 DC2.corp.contoso.com 存取設定的資訊。|  
|**1210**|DFSR|成功設定傳入複寫要求 RPC 其實 DFS 複寫服務。<br /><br />其他資訊：<br /><br />連接埠： 0 」|  
|**4614**|DFSR|在本機路徑 C:\Windows\SYSVOL\domain 初始化 SYSVOL DFS 複寫服務，並執行初始複寫正在等待。 複寫的資料夾會維持在初始同步狀態複寫其合作夥伴使用。 如果在升級為網域控制站伺服器，網域控制站不會通知和這個問題之前解析為網域控制站運作。 如果指定的合作夥伴也是在初始同步狀態下，或共用違規此伺服器上同步處理合作夥伴，這可能會發生。 如果發生 SYSVOL 移轉期間這個事件 DFS 複寫的檔案複製服務 (FRS)，請變更不會複寫出辨識這個問題之前。 這會造成 SYSVOL 資料夾變成與其他網域控制站不同步此伺服器上。<br /><br />其他資訊：<br /><br />複製的資料夾的名稱︰ SYSVOL 分享<br /><br />複製的資料夾 ID:*<GUID>*<br /><br />複寫群組的名稱︰ 網域系統磁碟區<br /><br />複寫群組 ID:*<GUID>*<br /><br />會員 ID:*<GUID>*<br /><br />唯讀模式： 0|  
|**4604**|DFSR|DFS 複寫服務順利初始化在本機路徑 C:\Windows\SYSVOL\domain SYSVOL 複寫資料夾。 這個成員已完成合作夥伴 dc1.corp.contoso.com SYSVOL 的初始同步的處理。若要檢查有任何 SYSVOL 共用，請打開在命令提示字元視窗，然後輸入「」網路共用」」。<br /><br />其他資訊：<br /><br />複製的資料夾的名稱︰ SYSVOL 分享<br /><br />複製的資料夾 ID:*<GUID>*<br /><br />複寫群組的名稱︰ 網域系統磁碟區<br /><br />複寫群組 ID:*<GUID>*<br /><br />會員 ID:*<GUID>*<br /><br />同步合作夥伴：*<domain controller FQDN>*|  
  
## <a name="BKMK_TshootVDCSafeRestore"></a>疑難排解模擬的網域控制站安全還原  
  
### <a name="tools-for-troubleshooting"></a>疑難排解工具  
  
#### <a name="logging-options"></a>登入選項  
建登的最重要的網域控制站安全快照還原問題的疑難排解工具。 所有的這些登的功能和最大的詳細資訊，預設設定。  
  
|||  
|-|-|  
|**操作**|**登入**|  
|**快照建立**|事件 viewer\Applications 和服務 logs\Microsoft\Windows\Hyper-V-背景工作|  
|**快照還原**|事件 viewer\Applications 和服務 logs\Directory 服務<br />事件 viewer\Windows logs\System<br />事件 viewer\Windows logs\Application<br />事件 viewer\Applications 和服務 logs\File 複寫服務<br />事件 viewer\Applications 和服務 logs\DFS 複寫<br />事件 viewer\Applications 和服務 logs\DNS<br />事件 viewer\Applications 和服務 logs\Microsoft\Windows\Hyper-V-背景工作|  
  
#### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>工具和疑難排解網域控制站設定的命令  
若要登不解釋問題的疑難排解，使用下列工具做為起點：  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   網路監視器 3.4  
  
#### <a name="BKMK_TshhotSafeRestore"></a>一般的網域控制站安全還原進行疑難排解的方法  
  
1.  安全快照還原預期，但有問題嗎？  
  
    1.  檢查服務 Directory 事件登入  
  
        1.  有快照還原錯誤嗎？  
  
        2.  有 AD 複寫錯誤嗎？  
  
    2.  檢查 [系統事件登入  
  
        1.  有通訊錯誤嗎？  
  
        2.  有 AD 錯誤嗎？  
  
2.  是安全的快照還原未預期嗎？  
  
    1.  檢查以判斷人員或造成回復 hypervisor 稽核登  
  
    2.  連絡 hypervisor 的所有系統管理員，並詢問它們至於人員復原 VM 不通知  
  
3.  伺服器實作 USN 復原保護並不安全還原嗎？  
  
    1.  檢查服務 Directory 事件登入不支援 hypervisor 或整合服務  
  
    2.  檢查作業系統，並驗證執行 Windows Server 2012 嗎？  
  
### <a name="BKMK_TshootSpecificSafeRestore"></a>特定的問題進行疑難排解  
  
#### <a name="events"></a>事件  
所有擬化檔案網域控制站安全快照還原事件寫入還原的網域控制站 VM Directory 服務事件登入。 應用程式，系統、 檔案複寫服務及 DFS 複寫事件登也可能包含失敗還原疑難排解有用的資訊。  
  
以下是 Windows Server 2012 安全還原特定的事件 Directory 服務事件登入。  
  
|||  
|-|-|  
|**事件編號**|**2170**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|警告|  
|**訊息**|偵測到代 ID 變更。<br /><br />代來電顯示快取中 DS （舊值）: %1<br /><br />代 ID 目前在 VM （新值）: %2<br /><br />一樣快照的應用程式、 一樣匯入操作後或即時移轉作業之後，就會發生代 ID 變更。 *<COMPUTERNAME>*將會建立新的叫用 ID 復原網域控制站。 應還原模擬的網域控制站使用一樣的快照。 支援的方法來還原或復原 content 的 Active Directory Domain Services 資料庫是還原所做的 Active Directory Domain Services 注意備份應用程式的系統狀態備份。|  
|**筆記與解析度**|如果需要快照，是成功事件。 否則，請檢查背景 V 於工作事件登入，或連絡 hypervisor 系統管理員。|  
  
|||  
|-|-|  
|**事件編號**|**2174**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|DC 是 virtual 網域控制站複製都還原 virtual 網域控制站開發進程的快照。|  
|**筆記與解析度**|實體網域控制站或模擬的網域控制站的快照不會還原的開始時的預期的事件|  
  
|||  
|-|-|  
|**事件編號**|**2181**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|交易中止因為一樣會還原到先前的狀態。  這是在一樣快照的應用程式、 一樣匯入進行操作之後，或之後即時移轉作業。|  
|**筆記與解析度**|還原快照時預期般運作。 交易追蹤 VM 代 ID 變更|  
  
|||  
|-|-|  
|**事件編號**|**2185**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|*<COMPUTERNAME>*停止 FRS 或 DFSR 所用服務複寫 SYSVOL 資料夾。<br /><br />服務的名稱︰ %1<br /><br />Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 *<COMPUTERNAME>*必須初始化未經授權還原 SYSVOL 複本本機上。 這會的停止 FRS 或 DFSR 服務使用複寫 SYSVOL 資料夾並開始它值觸發還原至適當登錄與執行。 重新開始 FRS 或 DFSR 服務時事件 2187年將會登入。|  
|**筆記與解析度**|還原快照時預期般運作。 在這個網域控制站所有 SYSVOL 資料會都取代合作夥伴 DC 的複本。|  
|**事件編號**|2186|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*若要停止 FRS 或 DFSR 所用服務複寫 SYSVOL 資料夾失敗。<br /><br />服務的名稱︰ %1<br /><br />錯誤碼: %2<br /><br />錯誤訊息: %3<br /><br />Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 *<COMPUTERNAME>*必須初始化未經授權還原 SYSVOL 複本本機上。 這是停止 FRS 或 DFSR 複寫服務使用複寫 SYSVOL 資料夾，然後將它開始值觸發還原至適當登錄與。 *<COMPUTERNAME>*無法停止目前執行的服務，且無法完成未經授權還原。 請手動執行未經授權還原。|  
|**筆記與解析度**|檢查 [系統、 FRS 及 DFSR 事件登的其他相關資訊。|  
  
|||  
|-|-|  
|**事件編號**|**2187**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|*<COMPUTERNAME>*開始使用複寫 SYSVOL 資料夾 FRS 或 DFSR 服務。<br /><br />服務的名稱︰ %1<br /><br />Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 *<COMPUTERNAME>*需要初始化未經授權還原 SYSVOL 複本本機上。 這是的停止 FRS 或 DFSR 服務使用複寫 SYSVOL 資料夾並開始它值觸發還原至適當登錄與。|  
|**筆記與解析度**|還原快照時預期般運作。 在這個網域控制站所有 SYSVOL 資料會都取代合作夥伴 DC 的複本。|  
  
|||  
|-|-|  
|**事件編號**|**2188**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*若要開始使用複寫 SYSVOL 資料夾 FRS 或 DFSR 服務失敗。<br /><br />服務的名稱︰ %1<br /><br />錯誤碼: %2<br /><br />錯誤訊息: %3<br /><br />Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 *<COMPUTERNAME>*需要初始化未經授權還原 SYSVOL 複本本機上。 這是的停止複寫 SYSVOL 用於 FRS 或 DFSR 服務或開頭適當的登錄和觸發還原值。 *<COMPUTERNAME>*開始 FRS 或 DFSR 所用服務將 SYSVOL 資料夾複寫失敗，無法完成未經授權還原。 請手動執行未經授權還原並重新開機服務。|  
|**筆記與解析度**|檢查 [系統、 FRS 及 DFSR 事件登的其他相關資訊。|  
  
|||  
|-|-|  
|**事件編號**|**2189**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|*<COMPUTERNAME>*設定 SYSVOL 複本初始化期間未經授權還原登錄下列值：<br /><br />登錄鍵: %1<br /><br />登錄值： %2<br /><br />登錄數值資料： %3<br /><br />Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 *<COMPUTERNAME>*需要初始化未經授權還原 SYSVOL 複本本機上。 這是的停止 FRS 或 DFSR 服務使用複寫 SYSVOL 資料夾並開始它值觸發還原至適當登錄與。|  
|**筆記與解析度**|還原快照時預期般運作。 在這個網域控制站所有 SYSVOL 資料會都取代合作夥伴 DC 的複本。|  
  
|||  
|-|-|  
|**事件編號**|**2190**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*無法設定 SYSVOL 複本初始化期間未經授權還原登錄下列值：<br /><br />登錄鍵: %1<br /><br />登錄值： %2<br /><br />登錄數值資料： %3<br /><br />錯誤碼: %4<br /><br />錯誤訊息: %5<br /><br />Active Directory 偵測到的裝載網域控制站一樣還原到先前的狀態。 *<COMPUTERNAME>*需要初始化未經授權還原 SYSVOL 複本本機上。 這是的停止 FRS 或 DFSR 服務使用複寫 SYSVOL 資料夾並開始它值觸發還原至適當登錄與。 *<COMPUTERNAME>*設定上述登錄值失敗，無法完成未經授權還原。 請手動執行未經授權還原。|  
|**筆記與解析度**|檢查應用程式和系統事件登。 調查第三方應用程式可能會封鎖登錄更新。|  
  
|||  
|-|-|  
|**事件編號**|**2200**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 *<COMPUTERNAME>*將目前的網域控制站初始化複寫。 完成複寫事件 2201年將會登入。|  
|**筆記與解析度**|還原快照時預期般運作。 標記輸入 AD 複寫的開頭。|  
  
|||  
|-|-|  
|**事件編號**|**2201**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 *<COMPUTERNAME>*已完成複寫將目前的網域控制站。|  
|**筆記與解析度**|還原快照時預期般運作。 標示輸入 AD 複寫的結尾。|  
  
|||  
|-|-|  
|**事件編號**|**2202**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 *<COMPUTERNAME>*將最新的網域控制站複寫失敗。 下一步定期複寫之後，將會更新的網域控制站。|  
|**筆記與解析度**|檢查 Directory 服務和系統事件登。 使用 repadmin.exe 嘗試強迫複寫，並記下的任何失敗。|  
  
|||  
|-|-|  
|**事件編號**|**2204**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|*<COMPUTERNAME>*偵測到一樣代收到的變更 變更表示 virtual 網域控制站的已經還原到先前的狀態。 *<COMPUTERNAME>*將會執行下列操作保護還原網域控制站分歧可能的資料免於和保護安全性主體的 Sid 重複建立：<br /><br />建立新的叫用 ID<br /><br />使目前 RID 集區<br /><br />將會在下一步輸入複寫驗證故障的擁有權。 這項視窗期間如果網域控制站保留 FSMO 的角色，該角色將無法使用。<br /><br />開始 SYSVOL 複寫服務還原操作。<br /><br />[開始]︰ 複寫將還原網域控制站帶到最新狀態。<br /><br />要求 RID 新集區。|  
|**筆記與解析度**|還原快照時預期般運作。 這解釋所有各種重設安全還原程序的一部分會發生的作業。|  
  
|||  
|-|-|  
|**事件編號**|**2205**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|*<COMPUTERNAME>*失效目前 RID 集區之後 virtual 網域控制站還原到先前的狀態。|  
|**筆記與解析度**|還原快照時預期般運作。 必須網域控制站有 travelled 的時間，他們可能有已經發出損壞本機 RID 集區。|  
  
|||  
|-|-|  
|**事件編號**|**2206**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*無法 virtual 網域控制站還原到先前的狀態後失效目前 RID 集區。<br /><br />其他資料：<br /><br />錯誤碼： %1<br /><br />錯誤值： %2|  
|**筆記與解析度**|檢查 Directory 服務和系統事件登。 確認移除主要 online 可以達到從此使用 Dcdiag.exe /test:ridmanager 伺服器|  
  
|||  
|-|-|  
|**事件編號**|**2207**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*無法還原之後 virtual 網域控制站還原到先前的狀態。 已要求到 DSRM 重新開機。 請檢查前的事件的詳細資訊。|  
|**筆記與解析度**|檢查 Directory 服務和系統事件登。|  
  
|||  
|-|-|  
|**事件編號**|**2208**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|資訊|  
|**訊息**|*<COMPUTERNAME>*刪除 DFSR 資料庫未經授權還原程序期間初始化 SYSVOL 複本。|  
|**筆記與解析度**|還原快照時預期般運作。 這可以確保 DFSR 非系統授權的合作夥伴俠同步 SYSVOL。 請注意，任何其他 DFSR 複寫資料夾 SYSVOL 所在的磁碟區上也非系統授權方式將會同步 （網域控制站不建議使用主機自訂 DFSR 設定 SYSVOL 所在的磁碟區）。|  
  
|||  
|-|-|  
|**事件編號**|**2209**|  
|**來源**|Microsoft-Windows-ActiveDirectory_DomainService|  
|**嚴重性**|錯誤|  
|**訊息**|*<COMPUTERNAME>*無法 delete DFSR 資料庫。<br /><br />其他資料：<br /><br />錯誤碼： %1<br /><br />錯誤值： %2<br /><br />Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 *<COMPUTERNAME>*需要初始化未經授權還原 SYSVOL 複本本機上。 適用於 DFSR，這是停止 DFSR 服務、 移除 DFSR 資料庫，與重新開始服務。 重新 DFSR 時將會重建資料庫，並開始初始同步。|  
|**筆記與解析度**|檢查 DFSR 事件登入。|  
  
#### <a name="error-messages"></a>錯誤訊息  
有任何直接互動式錯誤失敗模擬的網域控制站安全快照還原。所有複製資訊登 Directory 服務事件登入。 當然，任何重要的複寫或伺服器廣告錯誤的表現形式症狀其他地方。  
  
#### <a name="known-issues-and-support-scenarios"></a>已知的問題，以及案例的支援  
[適用於疑難排解網域控制站安全還原一般方法](../../../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md#BKMK_TshootSpecificSafeRestore)通常適合用來大部分的問題進行疑難排解。  
  
|||  
|-|-|  
|**問題**|**最近安全還原的網域控制站無法建立新的安全性原則**|  
|**症狀**|還原之後快照，嘗試使用的網域控制站無法建立新的安全性原則 （使用者、 電腦、 群組）：<br /><br />錯誤 0x2010<br /><br />Directory 服務無法配置相關的 id。|  
|**解析度和筆記**|還原的電腦的過時掌握移除主機 FSMO 的角色被因為此問題。 如果角色移到這個或其他網域控制站快照已拍攝的後再還原，還原的網域控制站不會有 RID 主版知識之前完成初始複寫。<br /><br />修正的相關問題，讓完成 AD 複寫還原的網域控制站輸入。 如果仍然無法運作，確認所有網域控制站都有之俠主控移除主機相同正確知識。|  
  
|||  
|-|-|  
|**問題**|**還原的網域控制站不要共用 SYSVOL 通知**|  
|**症狀**|還原快照之後, 一或多個 Dc 不執行動作通知、 不分享 sysvol，並不具有最新狀態 SYSVOL 內容|  
|**解析度和筆記**|DC 上游合作夥伴不具有正確複寫 DFSR 或 FRS 運作 SYSVOL 複本 無關安全還原此問題，但可能會顯示為安全還原的問題，請客戶不知道的影響未還原網域控制站的其他複寫問題，所以|  
  
### <a name="advanced-troubleshooting"></a>進階疑難排解  
這個模組設法教導進階疑難排解，透過*使用*登某些解釋所發生的範例，以。 如果您了解成功模擬的網域控制站作業的外觀，失敗明顯地在您的環境中。 這些登的出示來源的遞增訂單以*如預期般*事件相關複製的網域控制站在每個登入。  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-dfsr"></a>還原網域控制站為何使用 DFSR SYSVOL  
  
##### <a name="directory-services-event-log"></a>Directory 服務事件登入  
Directory 服務登入包含大部分操作資訊的安全還原。 Hypervisor 變更 VM 新一代 ID 及 NTDS 服務筆記它，然後失效 RID 集區變更叫用 id。 新 VM 新一代 ID 是設定和伺服器複製輸入廣告的資料。 DFSR 服務已停止和刪除裝載 SYSVOL 其資料庫，強迫未經授權的同步輸入。 調整 USN 高浮水印。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**2170**|ActiveDirectory_DomainService|偵測到代 ID 變更。<br /><br />代來電顯示快取中 DS （舊值）：<br /><br />*<number>*<br /><br />代目前中 ID VM （新值）：<br /><br />*<number>*<br /><br />一樣快照的應用程式、 一樣匯入操作後或即時移轉作業之後，就會發生代 ID 變更。 Active Directory Domain Services 會建立新的叫用 ID 復原網域控制站。 應還原模擬的網域控制站使用一樣的快照。 支援的方法來還原或復原 content Active Directory Domain Services 資料庫的是還原所做的 Active Directory Domain Services 注意備份應用程式的系統狀態備份。 」|  
|**2181**|ActiveDirectory_DomainService|交易中止因為一樣會還原到先前的狀態。  這是在一樣快照的應用程式、 一樣匯入進行操作之後，或之後即時移轉作業。|  
|**2204**|ActiveDirectory_DomainService|Active Directory Domain Services 偵測一樣代收到的變更 變更表示 virtual 網域控制站的已經還原到先前的狀態。 Active Directory Domain Services 將會執行下列操作保護還原網域控制站分歧可能的資料免於和保護安全性主體的 Sid 重複建立：<br /><br />建立新的叫用 ID<br /><br />使目前 RID 集區<br /><br />將會在下一步輸入複寫驗證故障的擁有權。 這項視窗期間如果網域控制站保留 FSMO 的角色，該角色將無法使用。<br /><br />開始 SYSVOL 複寫服務還原操作。<br /><br />[開始]︰ 複寫將還原網域控制站帶到最新狀態。<br /><br />要求 RID 新集區。 」|  
|**2181**|ActiveDirectory_DomainService|交易中止因為一樣會還原到先前的狀態。  這是在一樣快照的應用程式、 一樣匯入進行操作之後，或之後即時移轉作業。|  
|**1109**|ActiveDirectory_DomainService|已變更此 directory 伺服器呼叫識別碼屬性。 在建立備份之的時間的最高更新序號如下：<br /><br />呼叫識別碼屬性 （舊值）：<br /><br />*<GUID>*<br /><br />呼叫識別碼屬性 （新值）：<br /><br />*<GUID>*<br /><br />更新的序號：<br /><br />*<number>*<br /><br />呼叫識別碼 directory 伺服器還原備份媒體，從時變更設定為主控寫入應用程式 directory 磁碟分割、 已繼續一樣快照套用之後，在一樣匯入作業，或之後即時移轉作業。 應還原模擬的網域控制站使用一樣的快照。 還原或回復到支援的方法的 Active Directory Domain Services 資料庫 content 是還原進行備份 Active Directory Domain Services 感知應用程式的系統狀態備份。 」|  
|**2179**|ActiveDirectory_DomainService|MsDS-GenerationId 物件的屬性網域控制站的電腦已設定為下列參數：<br /><br />GenerationID 屬性：<br /><br />*<number>*|  
|**2200**|ActiveDirectory_DomainService|Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 Active Directory Domain Services 初始化複寫將目前的網域控制站。 完成複寫事件 2201年將會登入。|  
|**2201**|ActiveDirectory_DomainService|Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 完成將目前的網域控制站複寫 active Directory Domain Services。|  
|**2185**|ActiveDirectory_DomainService|Active Directory Domain Services 停止 FRS 或 DFSR 所用服務複寫 SYSVOL 資料夾。<br /><br />服務的名稱︰<br /><br />DFSR<br /><br />Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 Active Directory Domain Services 必須初始化未經授權還原 SYSVOL 複本本機上。 這會的停止 FRS 或 DFSR 服務使用複寫 SYSVOL 資料夾並開始它值觸發還原至適當登錄與執行。 事件 2187年將會登入重新開始 FRS 或 DFSR 服務時。 」|  
|**2208**|ActiveDirectory_DomainService|Active Directory Domain Services 刪除 DFSR 資料庫未經授權還原程序期間初始化 SYSVOL 複本。<br /><br />Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 Active Directory Domain Services 需要初始化未經授權還原 SYSVOL 複本本機上。 適用於 DFSR，這是停止 DFSR 服務、 移除 DFSR 資料庫，與重新開始服務。 重新 DFSR 時將會重建資料庫，並開始初始同步。"|  
|**2187**|ActiveDirectory_DomainService|Active Directory Domain Services 開始 FRS 或 DFSR 所用服務複寫 SYSVOL 資料夾。<br /><br />服務的名稱︰<br /><br />DFSR<br /><br />Active Directory 偵測到的裝載的網域控制站一樣還原到先前的狀態。 Active Directory Domain Services 需要初始化未經授權還原 SYSVOL 複本本機上。 這是的停止 FRS 或 DFSR 服務使用複寫 SYSVOL 資料夾並開始它值觸發還原至適當登錄與。 "|  
|**1587**|ActiveDirectory_DomainService|這項服務 directory 已還原或已設定為裝載應用程式 directory 磁碟分割。 如此一來，其複寫身分已變更。 合作夥伴要求複寫對使用我們舊的身分。 已經調整開始的序號。<br /><br />目的地 directory 服務下列物件 GUID 要求變更開始之前，在當地 directory 服務已從備份的媒體還原的 USN USN 相對應。<br /><br />物件 GUID:<br /><br />*<GUID> (<FQDN of partner domain controller>)*<br /><br />還原同時 USN:<br /><br />*<number>*<br /><br />如此一來，最新的向量的目的地 directory 服務已使用下列設定。<br /><br />先前資料庫 GUID:<br /><br />*<GUID>*<br /><br />先前的物件 USN:<br /><br />*<number>*<br /><br />先前屬性 USN:<br /><br />*<number>*<br /><br />新的 GUID 資料庫：<br /><br />*<GUID>*<br /><br />新的物件 USN:<br /><br />*<number>*<br /><br />新的屬性 USN:<br /><br />*<number>*|  
  
##### <a name="system-event-log"></a>系統事件登入  
系統事件登入記下的電腦提供離線一樣回 online 同步處理時間主機時，就會發生的時間。 RID 集區失效且 DFSR 或 FRS 服務會重新起始。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**1**|核心-一般|已變更系統時間，以*嗎？<now>* 從*< 快照時間日日期 >*。<br /><br />變更理由： 應用程式或系統元件變更的時間。|  
|**16654**|薩姆-directory-服務|已無效 account 識別碼 (Rid) 的集區。 這可能是因為預期如下：<br /><br />1.網域控制站是從備份還原。<br /><br />2.網域控制站執行一樣會還原的快照。<br /><br />3.系統管理員的身分，以手動方式有失效集區。<br /><br />查看 https://go.microsoft.com/fwlink/?LinkId=226247 如需詳細資訊。|  
|**7036**|服務控制管理員|DFS 複寫服務輸入停止的狀態。|  
|**7036**|服務控制管理員|DFS 複寫服務輸入執行的狀態。|  
  
##### <a name="application-event-log"></a>應用程式事件登入  
應用程式筆記 DFSR 資料庫停止並開始。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**103**|ESENT|\\\.\C:\System Volume Information\DFSR\database DFSRs (1360)*_<GUID>*\dfsr.db： 資料庫引擎停止 (0) 的執行個體。<br /><br />不正常關機： 0<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.000 [3]、 [4] 0.000、 0.141 [5]、 [6] 0.000、 0.000 [7]、 0.000 [8]、 [9] 0.000、 0.000 [10]、 0.016 [11]、 0.000 [12]、 [13] 0.000、 0.000 [14]、 0.000 [15]。|  
|**102**|ESENT|\\\.\C:\System Volume Information\DFSR\database DFSRs (532)*_<GUID>*\dfsr.db： 資料庫引擎 (6.02.8189.0000) 的新執行個體 (0) 開始。|  
|**105**|ESENT|\\\.\C:\System Volume Information\DFSR\database DFSRs (532)*_<GUID>*\dfsr.db： 資料庫引擎會開始新的執行個體 (0)。 (時間 = 0 秒)<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.000 [3]、 [4] 0.000、 0.000 [5]、 [6] 0.000、 0.000 [7]、 0.000 [8]、 [9] 0.031、 0.000 [10]、 [11] 0.000。|  
|||\\\.\C:\System Volume Information\DFSR\database DFSRs (532)*_<GUID>*\dfsr.db： 資料庫引擎建立新的資料庫 (\\\.\C:\System Volume Information\DFSR\database 1，*_<GUID>*\dfsr.db)。 (時間 = 0 秒)<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.016 [3]、 [4] 0.062、 [5] 0.000、 0.016 [6]、 [7] 0.000、 0.000 [8]、 [9] 0.015、 0.000 [10]、 0.000 [11]。|  
  
##### <a name="dfs-replication-event-log"></a>DFS 複寫事件登入  
DFSR 服務已停止和資料庫包含 SYSVOL 刪除，強迫未經授權的同步處理輸入。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**1006**|DFSR|停止 DFS 複寫服務。|  
|**1008**|DFSR|DFS 複寫服務已停止。|  
|**1002**|DFSR|開始工作 DFS 複寫服務。|  
|**1004**|DFSR|開始 DFS 複寫服務。|  
|**1314**|DFSR|DFS 複寫服務成功設定的偵錯登入的檔案。<br /><br />其他資訊：<br /><br />偵錯登入的檔案路徑： C:\Windows\debug|  
|**6102**|DFSR|DFS 複寫服務已成功已經登記完畢 WMI 提供者。|  
|**1206**|DFSR|DFS 複寫服務順利聯繫的網域控制站* <domain controller FQDN> *來存取設定的資訊。|  
|**1210**|DFSR|成功設定傳入複寫要求 RPC 其實 DFS 複寫服務。<br /><br />其他資訊：<br /><br />連接埠： 0|  
|**4614**|DFSR|在本機路徑 C:\Windows\SYSVOL\domain 初始化 SYSVOL DFS 複寫服務，並執行初始複寫正在等待。 複寫的資料夾會維持在初始同步狀態複寫其合作夥伴使用。 如果在升級為網域控制站伺服器，網域控制站不會通知和這個問題之前解析為網域控制站運作。 如果指定的合作夥伴也是在初始同步狀態下，或共用違規此伺服器上同步處理合作夥伴，這可能會發生。 如果發生 SYSVOL 移轉期間這個事件 DFS 複寫的檔案複製服務 (FRS)，請變更不會複寫出辨識這個問題之前。 這會造成 SYSVOL 資料夾變成與其他網域控制站不同步此伺服器上。<br /><br />其他資訊：<br /><br />複製的資料夾的名稱︰ SYSVOL 分享<br /><br />複製的資料夾 ID:*<GUID>*<br /><br />複寫群組的名稱︰ 網域系統磁碟區<br /><br />複寫群組 ID:*<GUID>*<br /><br />會員 ID:*<GUID>*<br /><br />唯讀模式： 0|  
|**4604**|DFSR|DFS 複寫服務順利初始化在本機路徑 C:\Windows\SYSVOL\domain SYSVOL 複寫資料夾。 這個成員已完成合作夥伴 dc1.corp.contoso.com SYSVOL 的初始同步的處理。若要檢查有任何 SYSVOL 共用，請打開在命令提示字元視窗，然後輸入「網路共用」。<br /><br />其他資訊：<br /><br />複製的資料夾的名稱︰ SYSVOL 分享<br /><br />複製的資料夾 ID:*<GUID>*<br /><br />複寫群組的名稱︰ 網域系統磁碟區<br /><br />複寫群組 ID:*<GUID>*<br /><br />會員 ID:*<GUID>*<br /><br />同步合作夥伴：*<partner domain controller FQDN>*|  
  
#### <a name="restoring-a-domain-controller-that-replicates-sysvol-using-frs"></a>還原網域控制站為何使用 FRS SYSVOL  
檔案複寫事件登入此時使用而不是 DFSR 事件登入。 應用程式也會將 FRS 相關的不同事件。 否則，Directory 服務和系統事件登入訊息通常都相同，並在同一個訂單以之前所述。  
  
##### <a name="file-replication-service-event-log"></a>檔案複製服務事件登入  
FRS 服務已停止，且重新起始非系統授權同步 SYSVOL D2 BURFLAGS 值。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**13502**|NTFRS|停止檔案複寫服務。|  
|**13503**|NTFRS|檔案複製服務已停止。|  
|**13501**|NTFRS|開始檔案複寫服務|  
|**13512**|NTFRS|複製檔案服務偵測到的磁碟機上的電腦 DC4 directory c:\windows\ntfrs\jet 讓的磁碟寫入快取。 檔案複寫服務可能無法修復磁碟機的電源中斷重大更新將會失去時。|  
|**13565**|NTFRS|檔案複寫服務初始化從另一部網域控制站的資料與系統磁碟區。 此程序完成之前電腦 DC4 無法加入網域控制站。 然後將會為 SYSVOL 共用系統磁碟區。<br /><br />若要檢查 SYSVOL 共用，請在命令提示字元中，輸入<br /><br />共用網路<br /><br />在檔案複寫服務完成初始設定程序，會出現 SYSVOL 共用。<br /><br />系統磁碟區的初始設定可能需要一些時間。 是的系統磁碟區，可供使用的其他網域控制站，以及複寫之間的時間間隔網域控制站的資料量而定。 」|  
|**13520**|NTFRS|將檔案複製服務移動既有的檔案* <path> *以* <path> *\NtFrs_PreExisting___See_EventLog。<br /><br />複製檔案服務可能 delete 中的檔案* <path>*隨時 \NtFrs_PreExisting___See_EventLog。 檔案可以複製全新儲存刪除從* <path> *\NtFrs_PreExisting___See_EventLog。 複製檔案到* <path> *如果已經有一些其他複製合作夥伴這些檔案名稱衝突可能導致。<br /><br />有時候，檔案複寫服務可能複製檔案的* <path> *\NtFrs_PreExisting___See_EventLog 到* <path> *而不是複製檔案的其他複製協力廠商。<br /><br />空間可以隨時復原刪除中的檔案的* <path> *\NtFrs_PreExisting___See_EventLog。|  
|**13553**|NTFRS|檔案複寫服務順利加入這台電腦複本下列設定：<br /><br />「 網域系統磁碟區 （SYSVOL 共用） 」<br /><br />這個事件和相關的資訊如下所示：<br /><br />電腦 DNS 名稱]*<domain controller FQDN>*]<br /><br />複本設定成員名稱]*<domain controller name>*]<br /><br />複本設定根路徑 」*<path>*]<br /><br />執行的路徑複本]* <path> * ]<br /><br />已正常運作的路徑複本]*<path>*]|  
|**13554**|NTFRS|檔案複寫服務順利新增連接到複本組如下：<br /><br />「 網域系統磁碟區 （SYSVOL 共用） 」<br /><br />從輸入]*<partner domain controller FQDN>*]<br /><br />輸出到]*<partner domain controller FQDN>*]<br /><br />詳細資訊，可能會出現在後續事件登入訊息。|  
|**13516**|NTFRS|檔案複寫服務不會再從成為網域控制站讓電腦 DC4。 已成功初始化系統磁碟區，已經通知 Netlogon 服務，系統磁碟區現在已經準備好 SYSVOL 共用。<br /><br />輸入 「 共用網路] 來檢查是否有 SYSVOL 共用。|  
  
##### <a name="application-event-log"></a>應用程式事件登入  
FRS 資料庫停止開始，並會清除 D2 BURFLAGS 作業。  
  
||||  
|-|-|-|  
|**事件編號**|**來源**|**訊息**|  
|**327**|ESENT|(1424) ntfrs 資料庫引擎分離資料庫 (1、 c:\windows\ntfrs\jet\ntfrs.jdb)。 (時間 = 0 秒)<br /><br />內部的時間順序： 0.000 [1] 0.015 [2]、 0.000 [3]、 [4] 0.000、 0.000 [5]、 0.516 [6]、 [7] 0.000、 0.000 [8]、 [9] 0.000、 0.000 [10]、 0.063 [11]、 [12] 0.000。<br /><br />恢復快取： 0|  
|**103**|ESENT|(1424) ntfrs 資料庫引擎停止 (0) 的執行個體。<br /><br />不正常關機： 0<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.000 [3]、 [4] 0.000、 0.000 [5]、 [6] 0.000、 0.000 [7]、 0.000 [8]、 0.031 [9]、 0.000 [10]、 0.016 [11]、 [12] 0.000、 0.000 [13]、 [14] 0.047、 0.000 [15]。|  
|**102**|ESENT|(3000) ntfrs 資料庫引擎 (6.02.8189.0000) 的新執行個體 (0) 開始。|  
|**105**|ESENT|(3000) ntfrs 資料庫引擎會開始新的執行個體 (0)。 (時間 = 0 秒)<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.000 [3]、 [4] 0.000、 [5] 0.000、 0.000 [6]、 [7] 0.000、 0.000 [8]、 [9] 0.062、 0.000 [10]、 0.141 [11]。|  
|**103**|ESENT|(3000) ntfrs 資料庫引擎停止 (0) 的執行個體。<br /><br />不正常關機： 0<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.000 [3]、 [4] 0.000、 0.000 [5]、 [6] 0.000、 0.000 [7]、 0.000 [8]、 0.000 [9]、 0.000 [10]、 0.000 [11]、 [12] 0.000、 0.015 [13]、 [14] 0.000、 0.000 [15]。|  
|**102**|ESENT|(3000) ntfrs 資料庫引擎 (6.02.8189.0000) 的新執行個體 (0) 開始。|  
|**105**|ESENT|(3000) ntfrs 資料庫引擎會開始新的執行個體 (0)。 (時間 = 0 秒)<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.000 [3]、 [4] 0.000、 [5] 0.000、 0.000 [6]、 [7] 0.000、 0.000 [8]、 [9] 0.078、 0.000 [10]、 0.109 [11]。|  
|**325**|ESENT|(3000) ntfrs 資料庫引擎建立新的資料庫 (1、 c:\windows\ntfrs\jet\ntfrs.jdb)。 (時間 = 0 秒)<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.016 [3]、 [4] 0.016、 [5] 0.000、 0.015 [6]、 [7] 0.000、 0.000 [8]、 [9] 0.078、 0.016 [10]、 0.000 [11]。|  
|**103**|ESENT|(3000) ntfrs 資料庫引擎停止 (0) 的執行個體。<br /><br />不正常關機： 0<br /><br />內部的時間順序： 0.000 [1] 0.000 [2]、 0.000 [3]、 [4] 0.000、 0.078 [5]、 [6] 0.000、 0.000 [7]、 0.000 [8]、 0.125 [9]、 0.016 [10]、 0.000 [11]、 [12] 0.000、 0.000 [13]、 [14] 0.000、 0.000 [15]。|  
|**102**|ESENT|(3000) ntfrs 資料庫引擎 (6.02.8189.0000) 的新執行個體 (0) 開始。|  
|**105**|ESENT|(3000) ntfrs 資料庫引擎會開始新的執行個體 (0)。 (時間 = 0 秒)<br /><br />內部的時間順序： 0.016 [1] 0.000 [2]、 0.000 [3]、 [4] 0.094、 [5] 0.000、 0.000 [6]、 [7] 0.000、 0.000 [8]、 [9] 0.032、 0.000 [10]、 0.000 [11]。|  
|**326**|ESENT|(3000) ntfrs 資料庫引擎連接資料庫 (1、 c:\windows\ntfrs\jet\ntfrs.jdb)。 (時間 = 0 秒)<br /><br />內部的時間順序： 0.000 [1] 0.015 [2]、 0.000 [3]、 [4] 0.000、 0.016 [5]、 0.015 [6]、 [7] 0.000、 0.000 [8]、 [9] 0.000、 0.000 [10]、 0.000 [11]、 [12] 0.000。<br /><br />儲存快取︰ 1|  
  


