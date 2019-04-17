---
ms.assetid: 5ab76733-804d-4f30-bee6-cb672ad5075a
title: "疑難排解網域控制站部署"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 254209b0da6a7bc0cd5f3880e14a7e5b16822cdd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-domain-controller-deployment"></a>疑難排解網域控制站部署

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題涵蓋詳細的網域控制站設定和部署疑難排解的方法。  
  
-   [疑難排解簡介](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Intro)  
  
-   [疑難排解選項](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Options)  
  
## <a name="BKMK_Intro"></a>疑難排解簡介  
![疑難排解](media/Troubleshooting-Domain-Controller-Deployment/adds_deploy_troubleshooting.png)  
  
## <a name="BKMK_Options"></a>疑難排解選項  
  
### <a name="logging-options"></a>登入選項  
建登的樂器最重要的網域控制站升級降級的問題進行疑難排解。 這些登的所有功能，而且預設設定為最大的詳細資訊。  
  
|階段|登入|  
|---------|-------|  
|伺服器管理員或 ADDSDeployment Windows PowerShell 作業|-%systemroot%\debug\dcpromoui.log<br /><br />-%systemroot%\debug\dcpromoui*.log|  
|安裝日 Promotion 網域控制站|-%systemroot%\debug\dcpromo.log<br /><br />-%systemroot%\debug\dcpromo*.log<br /><br />事件 viewer\Windows logs\System<br /><br />事件 viewer\Windows logs\Application<br /><br />事件 viewer\Applications 和服務 logs\Directory 服務<br /><br />事件 viewer\Applications 和服務 logs\File 複寫服務<br /><br />事件 viewer\Applications 和服務 logs\DFS 複寫|  
|樹系或網域升級|-%systemroot%\debug\adprep\\<datetime>\adprep.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\csv.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\dspecup.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\ldif.log*|  
|伺服器管理員 ADDSDeployment Windows PowerShell 部署引擎|事件 viewer\Applications 和服務 logs\Microsoft\Windows\DirectoryServices-Deployment\Operational|  
|Windows 維護|-%systemroot%\Logs\CBS\\*<br /><br />-%systemroot%\servicing\sessions\sessions.xml<br /><br />-%systemroot%\winsxs\poqexec.log<br /><br />-%systemroot%\winsxs\pending.xml|  
  
### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>工具和疑難排解網域控制站設定的命令  
若要登不解釋問題的疑難排解，使用下列工具做為起點：  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   [AutoRuns.exe](https://technet.microsoft.com/sysinternals/bb963902.aspx)、工作管理員] 中，並 MSInfo32.exe  
  
-   [網路監視器 3.4](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=4865)（或第三方網路擷取和分析工具）  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>一般的網域控制站設定進行疑難排解的方法  
  
1.  是否有簡單語法問題會造成錯誤？  
  
    1.  您輸入錯誤或忘記 ADDSDeployment Windows PowerShell 來提供引數嗎？ 例如，如果使用 ADDSDeployment Windows PowerShell，忘記新增所需的引數**的網域名稱**及有效的名稱嗎？  
  
    2.  檢查仔細以查看完全它失敗的原因剖析命令列所提供的 Windows PowerShell 主控台輸出。  
  
2.  必要條件的失敗是錯誤？  
  
    1.  必要條件檢查現在無法用來顯示為嚴重促銷結果許多錯誤。  
  
    2.  必要條件錯誤的文字仔細地檢查，他們提供所需的指導方針解析大部分的問題，只要在受控制的案例。  
  
3.  是在升級，因此嚴重錯誤？  
  
    1.  仔細地檢查結果：許多錯誤有例如輸入錯誤的密碼、網路名稱的解析度或重大離線網域控制站簡單解釋。  
  
    2.  檢查的 Dcpromoui.log 與 dcpromo.log 的輸出] 中顯示的錯誤則運作向後，才能看見指示發生失敗的原因。  
  
        1.  隨時比較正常運作的範例登入  
  
        2.  才結果表示延伸架構或準備的樹系或網域有問題，請檢查有錯誤 ADPrep 登。  
  
        3.  只有 Dcpromoui.log 缺少詳細資料，或結束任意因為例外未在設定程序，請檢查有錯誤對部署事件登入。  
  
    3.  檢查其他設定問題的指標 Directory 服務、系統和應用程式事件登。 通常時間網域控制站升級是只會影響所有分散式的系統其他網路設定錯誤的問題。  
  
    4.  使用 dcdiag.exe 和 repadmin.exe 驗證整體的樹系健康狀態，表示細微錯誤，可能會使進一步網域控制站升級設定。  
  
    5.  使用 AutoRuns.exe、工作管理員] 中或 MSinfo32.exe 來檢查電腦的協力廠商軟體可能會干擾。  
  
        1.  移除協力廠商軟體（不只會停用「軟體」; 不會阻止載入驅動程式）。  
  
    6.  無法升級，以及複寫合作夥伴網域控制站和分析升級程序與雙面網路擷取的電腦上安裝 NetMon 3.4。  
  
        1.  比較此工作 lab 環境了解良好的升級的外觀及失敗的位置。  
  
        2.  此時的樹系物件、變更預設的安全性，或網路時，可能會錯誤，而且這個新的網域控制站買錯誤設定 DNS、防火牆、主機入侵防護軟體，或其他外因素而有所不同。  
  
### <a name="troubleshooting-specific-problems"></a>特定的問題進行疑難排解  
  
#### <a name="events-and-error-messages"></a>事件及錯誤訊息  
網域控制站升級並隨時降級傳回結尾的作業，並不大多數程式中，像執行無法退貨零成功的程式碼。 若要查看結尾的網域控制站設定的程式碼，您有數個選項：  
  
1.  當使用伺服器管理員中，檢查促銷結果中自動重新開機前 10 秒。  
  
2.  當使用 ADDSDeployment Windows PowerShell，請檢查促銷結果中自動重新開機前 10 秒。 或者，選擇不要在下載完成度時自動重新開機。 您應該會新增**格式清單**以方便朗讀輸出管線。 例如：  
  
    ```  
    Install-addsdomaincontroller <options> -norebootoncompletion:$true | format-list  
  
    ```  
  
    必要條件驗證，驗證錯誤不會繼續放以重新開機，讓他們會顯示在所有的案例。 例如：  
  
  ![疑難排解](media/Troubleshooting-Domain-Controller-Deployment/ADDS_PSPrereqError.png)  
  
3.  在任何案例中，檢查 dcpromo.log 和 dcpromoui.log。  
  
    > [!NOTE]  
    > 一些的錯誤，以下是不會再可能因為較新的作業系統中的作業系統和網域控制站設定變更。 新的 ADDSDeployment Windows PowerShell 代碼也會防止特定錯誤，但 dcpromo.exe//unattend 不;這是另一個切換所有已取代帶領 ADDSDeployment Windows PowerShell 來您目前自動化的理由。  
  
促銷和降級返回下列成功郵件的驗證碼。  
  
|錯誤碼|解釋|注意|  
|--------------|---------------|--------|  
|1|結束成功|您仍必須重新開機，這只是資訊自動重新旗標已移除|  
|2|結束，成功，必須重新開機||  
|3|結束的成功、失敗嚴重的|通常會看到時傳回 DNS 委派警告。 如果未設定 DNS 委派，使用：<br /><br />-creatednsdelegation: $false|  
|4|結束時，使用嚴重的錯誤的成功、需要重新開機|通常會看到時傳回 DNS 委派警告。 如果未設定 DNS 委派，使用：<br /><br />-creatednsdelegation: $false|  
  
促銷和降級返回下列失敗郵件的驗證碼。 另外還有可能延伸的錯誤訊息。隨時朗讀整個錯誤仔細，而不只的數字的部分。  
  
|錯誤碼|解釋|建議的解析度|  
|--------------|---------------|------------------------|  
|11|已執行網域控制站升級|無法執行一個執行個體的網域控制站升級一次相同的目標電腦比|  
|12|使用者必須是系統管理員|以系統管理員群組並確定您的 UAC 提高建成員登入|  
|13|憑證授權單位已安裝|您無法降級這個網域控制站，也很憑證授權單位。 不要的移除 CA 之前您仔細庫存其使用方式-如果這發行憑證，移除的角色，會導致服務中斷。 建議您執行的網域控制站 Ca|  
|14|在 [安全開機模式執行|伺服器開機進入標準模式|  
|15|角色變更已進行中] 或 [需要重新開機|您必須重新開機（因為先前的設定變更）升級之前|  
|16|執行錯誤平台|*不可能會收到這個錯誤訊息*|  
|17|不 NTFS 5 磁碟機存在|這個錯誤不能在 Windows Server 2012，這需要至少 %系統磁碟機格式化為 NTFS|  
|18|在 [windir 空間不足|免費使用 cleanmgr.exe %系統磁碟機 %磁碟區上的空間|  
|19|名稱變更為擱置中、需要重新開機|重新開機伺服器|  
|20|電腦名稱是語法|重新命名電腦的有效的名稱|  
|21|這個網域控制站保留故障、是 GC、和/或是 DNS 伺服器|新增**-demoteoperationmasterrole**使用**-forceremoval**。|  
|22|TCP/IP 必須安裝或無法運作|檢查電腦有 TCP/IP 設定，繫結，且可正常運作|  
|23|DNS client 必須第一次設定|加入網域新的網域控制站時設定的主要 DNS 伺服器|  
|24|提供的認證會是無效或遺失必要的項目|確認您的使用者名稱和密碼正確無誤|  
|25|找不到指定的網域網域控制站|驗證免 DNS client 設定|  
|26|無法從樹系讀取的網域清單|驗證 DNS client 設定、免 LDAP 功能|  
|27|遺失的網域名稱|指定當中學網域|  
|28|錯誤的網域名稱|升級時，請選擇其他、有效的 DNS 網域名稱|  
|29|家長網域不會存在|請確認建立新的子女網域或樹網域時指定父系網域|  
|30|不在森林中的網域|提供驗證的網域名稱|  
|31|子女網域已存在|指定不同的網域名稱|  
|32|錯誤 NetBIOS 的網域名稱|指定有效 NetBIOS 網域名稱|  
|33|IFM 檔案的路徑不正確|驗證您的路徑，從媒體安裝資料夾|  
|34|IFM 資料庫是錯誤|使用正確安裝的媒體此作業系統和角色（相同的作業系統版本、相同類型的網域控制站-與 RWDC RODC）|  
|35|遺失 SYSKEY|從媒體安裝已加密，您必須提供有效的 SYSKEY 使用|  
|37|路徑 NTDS 資料庫或其登不正確|修正的 NTFS 磁碟區，不對應的磁碟機或底色變更資料庫和登的路徑|  
|38|磁碟區不是針對 NTDS 資料庫或登空間不足|釋出空間使用 cleanmgr.exe、新增更多磁碟空間，以手動方式清除空間其他地方移動不必要的資料|  
|39|路徑 SYSVOL 不正確|變更路徑 SYSVOL 資料夾的 NTFS 修正磁碟區，不對應的磁碟機或底色|  
|40|無效的網站名稱|提供存在網站名稱|  
|41|必須指定密碼的安全模式|提供密碼 DSRM 帳號，並無法空白不論密碼原則設定的方式|  
|42|安全模式密碼不符合條件（僅限促銷）|符合密碼的原則設定的規則 DSRM account 提供的密碼|  
|43|系統管理員密碼不符合條件（僅限降級）|提供的密碼本機系統管理員 account 符合密碼的原則設定規則|  
|44|無效的樹系指定的名稱|指定有效的樹系根 DNS 網域名稱|  
|45|樹系指定名稱已存在|選擇不同的樹系根 DNS 網域名稱|  
|46|無效的名稱指定樹|指定樹有效的 DNS 網域名稱|  
|47|樹指定名稱已存在|選擇不同的樹 DNS 網域名稱|  
|48|樹名稱並不適用於樹系結構|選擇不同的樹 DNS 網域名稱|  
|49|指定的網域不會存在|確認您的輸入的網域名稱|  
|50|在降級，最後一個網域控制站偵測到即使在不是，或指定了一個網域控制站，但不是|未指定**網域中的最後一個網域控制站**(**-lastdomaincontrollerindomain**) 除非它。 使用**-ignorelastdcindomainmismatch**若要覆寫如果其實這是最後一個的網域控制站且虛設網域控制站中繼資料|  
|51|在這個網域控制站的應用程式的磁碟分割存在|若要指定**移除應用程式的磁碟分割**(**-removeapplicationpartitions**)|  
|52|需要找不到命令列引數（也就是回應檔案必須指定命令列上）|*僅限看過的帶領//unattend，這會取代。 查看較舊的文件*|  
|53|促銷日降級失敗，電腦必須重新開機以清理|檢查登和延伸的錯誤|  
|54|促銷日降級失敗|檢查登和延伸的錯誤|  
|55|促銷日降級使用者已取消|檢查登和延伸的錯誤|  
|56|促銷日降級被取消使用者，若要清除的電腦必須重新開機|檢查登和延伸的錯誤|  
|58|在 RODC 升級期間必須指定網站的名稱|您必須 RODC 指定網站，將不會自動偵測 RWDC 類似|  
|59|降級，在這個網域控制站為其區域的其中一個的最後一個 DNS 伺服器|指定這是**網域中的最後一個 DNS 伺服器**，或使用**-ignorelastdnsserverfordomain**|  
|60|執行 Windows Server 2008，或較新的網域控制站必須以升級 RODC 網域中出現|促銷至少一個 Windows Server 2008 或更新版本模型寫入網域控制站|  
|61|您無法使用 DNS 現有不裝載 DNS 網域中安裝 Active Directory Domain Services|*不可能會收到這個錯誤訊息*|  
|62|回應檔案不會有 [DCInstall] 區段|*僅限看過的帶領//unattend，這會取代。 查看較舊的文件。*|  
|63|樹系正常運作的電量低於 windows server 2003|森林功能提高到至少 Windows Server 2003 原生。 Windows 2000 及 Windows nt4.0 已不再支援的作業系統|  
|64|促銷元件二進位偵測失敗無法|安裝 AD DS 角色|  
|65|促銷元件二進位安裝失敗無法|安裝 AD DS 角色|  
|66|促銷作業系統偵測失敗無法|檢查延伸的錯誤和登;伺服器傳回的作業系統版本失敗。 電腦將需要重新安裝，因為它的整體健康是高度可疑有可能|  
|68|複製合作夥伴不正確|使用 repadmin.exe 或**取得-ADReplication\ *** Windows PowerShell 來驗證合作夥伴網域控制站健康|  
|69|需要連接埠已經在使用透過其他應用程式|使用**netstat.exe-anob**尋找 [處理程序，不正確已指派給保留 AD DS 連接埠|  
|70|森林根網域控制站必須 GC|*僅限看過的帶領//unattend，這會取代。 查看較舊的文件*|  
|71|已安裝的 DNS 伺服器|未安裝 DNS 指定 (**-installDNS**) 如果已安裝的 DNS 服務|  
|72|電腦正在執行非系統管理員模式遠端桌面服務|您無法將這個網域控制站，升級為它也是設定為兩個以上系統管理員使用者 RDS 伺服器。 不要的移除 RDS 之前是使用的應用程式或使用者，請移除仔細庫存其使用方式-會導致發生停電|  
|73|無效的功能指定的樹系的層級。|指定有效的樹系功能層級|  
|74|無效的指定的網域功能層級。|指定有效的網域功能層級|  
|75|無法判斷複寫預設密碼的原則。|驗證 RODC 密碼複寫原則存在，以及可以存取|  
|76|指定複製/非複寫安全性群組不正確|驗證您輸入正確的網域和使用者帳號中指定複寫密碼原則時|  
|77|無效的指定引數|檢查登和延伸的錯誤|  
|78|若要檢查 Active Directory 樹系失敗|檢查登和延伸的錯誤|  
|79|無法升級 RODC，因為 rodcprep 尚未執行|使用 Windows Server 2012 準備樹系或使用**adprep.exe /rodcprep**|  
|80|準備網域尚未執行|使用 Windows Server 2012 準備網域，或使用**adprep.exe /domainprep**|  
|81|Forestprep 尚未執行|使用 Windows Server 2012 準備樹系或使用**adprep.exe /forestprep**|  
|82|森林架構不相符|使用 Windows Server 2012 準備樹系或使用**adprep.exe /forestprep**|  
|83|不支援的 SKU|*不可能會收到這個錯誤訊息*|  
|84|無法偵測到的網域控制站 account|驗證現有的網域控制站具有正確的使用者 account 控制屬性設定。|  
|85|無法選取網域控制站 account 第 2 階段|如果指定」使用現有 Account」，但找到可能不帳號，或是錯誤 account 查詢時傳回。 請確定您所提供的正確 RODC 暫存 account|  
|86|需要執行「步驟 2 升級|如果您宣傳的其他網域控制站但現有 account 存在，「允許重新安裝「並未指定傳回|  
|87|衝突類型的網域控制站 account 存在|升級之後，如果不是嘗試附加至位置的網域控制站之前將電腦重新命名。 您必須將它附加到位置的網域控制站 account 使用**-useexistingaccount**，並正確唯讀或寫入引數，根據 account 類型|  
|88|指定的伺服器管理員不正確|指定 RODC 管理委派無效負責。 請確認指定 account 是正確的使用者或群組|  
|89|指定網域 RID 的主機已離線。|使用**netdom.exe 查詢 fsmo**來偵測 RID 主機。 將它 online 並讓它更容易存取升級您的網域控制站|  
|90|網域命名主機是離線。|使用**netdom.exe 查詢 fsmo**來偵測網域命名主機。 將它 online 並讓它更容易存取升級您的網域控制站|  
|91|無法偵測程序是否 wow64|*收到這個錯誤可能無法再，作業系統為 64 位元*|  
|92|不支援 Wow64 處理程序|*收到這個錯誤可能無法再，作業系統為 64 位元*|  
|93|適用於非讓降級不執行網域控制站服務|[開始] 的 AD DS 服務|  
|94|本機系統管理員密碼不符合需求：空白，或者不需要|提供非空白的密碼，並確認密碼本機原則需要密碼|  
|95|無法降級最後一個 Windows Server 2008 或較新的網域控制站所在動態 Rodc 網域中|您可以在所有 Windows Server 2008 或更新版本寫入網域控制站都降級之前，您必須先都降級所有 Rodc|  
|96|無法解除安裝 DS 二進位檔|*僅限看過的帶領//unattend，這會取代。 查看較舊的文件*|  
|97|森林功能層級版本高於子女網域作業系統|提供子女網域功能相同或更高的樹系功能層級|  
|98|正在進行元件二進位安裝/解除安裝。|*僅限看過的帶領//unattend，這會取代。 查看較舊的文件*|  
|99|森林功能層級是太低（錯誤是 Windows Server 2012 只）|森林功能提高到至少 Windows Server 2003 原生。 Windows 2000 及 Windows nt4.0 已不再支援的作業系統|  
|100|網域功能層級是太低（錯誤是 Windows Server 2012 只）|至少提高網域功能等級以 Windows Server 2003 原生。 Windows 2000 及 Windows nt4.0 已不再支援的作業系統|  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>已知日可能的問題與支援案例  
以下是在 Windows Server 2012 開發程序期間常見的問題。 這些問題」的設計」，並具有有效的因應措施或更適合技巧，以避免使用它們首先。 有許多這些問題的是 Windows Server 2008 R2 和較舊的作業系統，在相同，但重新寫入部署 AD DS 的帶來增強的敏感度問題。  
  
|問題|降級網域控制站離開 DNS 執行的不區域|  
|---------|-----------------------------------------------------------------|  
|症狀|伺服器仍然 DNS 要求回應，但不區域資訊|  
|解析度和筆記|移除時 AD DS 角色，也會移除 DNS 伺服器角色或 DNS 伺服器服務設定已停用。 請記得 DNS client 指向比本身另一部伺服器。 如果您使用 Windows PowerShell 之後您降級伺服器, 執行：<br /><br />程式碼-解除安裝 windowsfeature dns<br /><br />或<br /><br />程式碼-設定服務 dns 中停用<br />停止服務 dns|  
  
|問題|Windows Server 2012 升級現有的單一標籤網域到不會設定 updatetopleveldomain = 1 或 allowsinglelabeldnsdomain = 1 台|  
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|  
|症狀|不會發生 DNS 動態記錄登記|  
|解析度和筆記|設定使用群組原則 Netlogon 和 DNS 這些值。 Microsoft 開始封鎖單一標籤網域建立 Windows Server 2008;若要變更為 [已核准 DNS 網域結構，您可以使用 ADMT 或網域重新命名工具。|  
  
|問題|如果有預先建立、位置 RODC 帳號，便會失敗網域中的最後一個網域控制站降級|  
|---------|------------------------------------------------------------------------------------------------------------|  
|症狀|降級失敗，並訊息：<br /><br />**Dcpromo.General.54**<br /><br />Active Directory Domain Services 找不到另一個 Active Directory 網域控制站傳輸 directory 磁碟分割 DATA-CN 剩餘資料 = 區結構描述 DATA-CN = 設定，俠 = corp，俠 = contoso 俠 = com。<br /><br />「指定的網域名稱的格式不正確的「。|  
|解析度和筆記|移除任何剩餘預先之前降級網域中建立 RODC 帳號使用**Dsa.msc**或**Ntdsutil.exe 中繼資料清理]**。|  
  
|問題|自動樹系和網域準備不會執行 GPPREP|  
|---------|---------------------------------------------------------------|  
|症狀|適用於群組原則，結果設定的原則 (RSOP) 計劃模式跨網域計劃功能需要現有 GP 系統更新的檔案和 Active Directory 權限。 Gpprep，而您無法使用 RSOP 規劃跨網域。|  
|解析度和筆記|執行**adprep.exe /gpprep**以手動方式的所有先前已未準備適用於 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的網域。 系統管理員歷史加入網域的每個升級不應該執行 GPPrep 一次。 因為您已經設定自訂的適當權限，如果它會導致所有 SYSVOL 內容重新複寫所有網域控制站它不是執行來自動 adprep。|  
  
|問題|從媒體安裝驗證時指向底色失敗|  
|---------|------------------------------------------------------------------|  
|症狀|傳回錯誤：<br /><br />程式碼-無法驗證媒體路徑。 例外呼叫」GetDatabaseInfo」與「2「引數。 無效的資料夾。|  
|解析度和筆記|您必須儲存在本機磁碟機，不遠端底色 IFM 檔案。 此刻意封鎖會防止部分伺服器促銷因為網路中斷。|  
  
|問題|顯示期間網域控制站提升兩次 DNS 委派警告|  
|---------|-------------------------------------------------------------------------|  
|症狀|退貨警告*兩次*當升級使用 ADDSDeployment Windows PowerShell:<br /><br />程式碼 –「無法建立委派此 DNS 伺服器，因為授權家長區域找不到或無法執行 Windows DNS 伺服器。 如果您使用現有的基礎結構 DNS 整合，您應該父區確保可靠的名稱解析從網域以外的來手動建立委派給此 DNS 伺服器。 否則，不不需要任何動作。」|  
|解析度和筆記|略過。 ADDSDeployment Windows PowerShell 顯示的第一次在必要的檢查，然後再試一次時設定的網域控制站的警告。 如果您不想要設定 DNS 委派，使用引數：<br /><br />程式碼--creatednsdelegation: $false<br /><br />執行*未*隱藏此訊息以跳過的必要條件檢查|  
  
|問題|在設定期間指定 UPN 或非網域認證傳回誤導錯誤|  
|---------|--------------------------------------------------------------------------------------------|  
|症狀|伺服器管理員會傳回錯誤：<br /><br />程式碼-例外呼叫」DNSOption」與「6「引數<br /><br />ADDSDeployment Windows PowerShell 傳回錯誤：<br /><br />失敗碼-驗證使用者權限。 您必須提供此帳號所屬的網域名稱。|  
|解析度和筆記|請確定您提供有效的網域認證的形式**網域使用者**。|  
  
|問題|移除使用 Dism.exe 對-DomainController 角色會導致無法開機伺服器|  
|---------|---------------------------------------------------------------------------------------------------|  
|症狀|如果使用 Dism.exe 適當降級網域控制站之前，請先移除 AD DS 角色，不再伺服器通常會開機，並顯示錯誤：<br /><br />程式碼-狀態：0x000000000<br />資訊：已發生意外的錯誤。|  
|解析度和筆記|開機 Directory 服務修復模式使用*shift 鍵 + F8*。 新增 AD DS 角色，以及強制降級網域控制站。 或者，從備份還原系統狀態。 請勿使用 Dism.exe AD DS 角色移除。公用程式，並不知道的網域控制站。|  
  
|問題|當 forestmode 設 Win2012 安裝新的樹系失敗|  
|---------|--------------------------------------------------------------------|  
|症狀|促銷使用 ADDSDeployment Windows PowerShell 傳回錯誤：<br /><br />程式碼-Test.VerifyDcPromoCore.DCPromo.General.74<br /><br />必要條件網域控制站促銷驗證失敗。 無效的指定的網域功能層級|  
|解析度和筆記|未指定 Win2012 而不需要的樹系功能模式*也*指定 Win2012 網域功能模式。 以下是可將不會出現錯誤範例：<br /><br />程式碼--forestmode Win2012-domainmode Win2012]|  
  
|||  
|-|-|  
|問題|按一下 [驗證中安裝媒體選取項目] 區域中的顯示以不執行任何動作|  
|症狀|當您指定的路徑 IFM 資料夾時，按一下**確認**按鈕永遠不會傳回訊息，或是執行任何動作。|  
|解析度和筆記|**確認**按鈕只會傳回錯誤如果有問題。 否則，它讓**下一步**按鈕，可選取，如果您有提供 IFM 路徑。 您必須按**確認**如果您有選取 IFM 繼續。|  
  
|||  
|-|-|  
|問題|降級使用伺服器管理員中不提供意見反應，直到完成。|  
|症狀|當使用移除 AD DS 角色與降級網域控制站伺服器管理員，就會提供降級完成，或是失敗之前不持續提供意見。|  
|解析度和筆記|這是一項限制伺服器管理員。 意見反應，請使用 ADDSDeployment Windows PowerShell cmdlet:<br /><br />程式碼-Uninstall-addsdomaincontroller|  
  
|||  
|-|-|  
|問題|從驗證媒體安裝不會偵測提供寫入網域控制站，或相反該 RODC 媒體。|  
|症狀|[驗證] 按鈕重點新網域控制站使用 IFM 並提供 IFM 不正確的媒體-例如寫入網域控制站的 RODC 媒體或 RODC RWDC 媒體-不會傳回錯誤。 之後，錯誤而失敗促銷：<br /><br />程式碼-為網域控制站設定此電腦嘗試時發生錯誤。 <br />Install-From-Media 升級 Read-Only 俠不得因為不能指定的來源資料庫。 只從其他 Rodc 資料庫可用於 RODC IFM 升級。|  
|解析度和筆記|請確認只會驗證 IFM 的整體完整性。 不提供伺服器錯誤 IFM 類型。 再試一次使用正確的媒體升級之前，請重新伺服器。|  
  
|||  
|-|-|  
|問題|升級 RODC 預先建立的電腦將會失敗|  
|症狀|使用 ADDSDeployment Windows PowerShell 來提升電腦分段的 account 新 RODC 時, 收到錯誤訊息：<br /><br />程式碼-無法解析使用名參數指定參數設定。    <br />InvalidArgument: ParameterBindingException<br />    + FullyQualifiedErrorId: AmbiguousParameterSet Microsoft.DirectoryServices.Deployment.PowerShell.Commands.Install|  
|解析度和筆記|不提供已經預先建立 RODC 帳號已經定義的參數。 這些功能包括：<br /><br />程式碼--readonlyreplica<br />-installdns<br />-donotconfigureglobalcatalog<br />-站台名稱<br />-installdns|  
  
|||  
|-|-|  
|問題|取消選取日選取 [自動重新開機每個項目的伺服器必要「無法執行任何動作|  
|症狀|如果選取（或選取 [不）伺服器管理員選擇**必要時自動重新開機每個項目的伺服器**whendemoting 透過角色移除網域控制站，伺服器一律重新開機，無論的選擇。|  
|解析度和筆記|這是刻意。 降級程序重新開機伺服器這個設定。|  
  
|||  
|-|-|  
|問題|Dcpromo.log 顯示 [[錯誤] 的安全性設定伺服器檔案無法使用 2」|  
|症狀|網域控制站降級完成不問題，但檢查帶領登入顯示錯誤：<br /><br />2 失敗碼-[錯誤] 設定伺服器的檔案安全性|  
|解析度和筆記|略過、錯誤，預期和色彩。|  
  
|||  
|-|-|  
|問題|必要條件 adprep 核取失敗，錯誤「無法執行換貨架構衝突檢查]|  
|症狀|當您嘗試升級到現有的 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的樹系的 Windows Server 2012 網域控制站時, 錯誤而失敗必要條件檢查：<br /><br />失敗碼-的必要條件 AD 準備的確認。 無法 Exchange 架構衝突檢查執行為網域*<domain name>* (例外：不適 RPC 伺服器)<br /><br />Adprep.log 顯示錯誤：<br /><br />程式碼-Adprep 無法擷取的資料，從伺服器*<domain controller>*<br /><br />透過 Windows 管理檢測 (WMI)。|  
|解析度和筆記|新的網域控制站無法存取 WMI 透過向現有的網域控制站 DCOM 日 RPC 通訊協定。 若要到目前為止，已經有三個原因：<br /><br />現有的網域控制站-防火牆規則封鎖存取<br /><br />從「登入即服務」不見-網路服務 account (SeServiceLogonRight) 現有的網域控制站的權限<br /><br />-NTLM 上已停用網域控制站、使用中所述的安全性原則[簡介限制 NTLM 驗證](https://technet.microsoft.com/library/dd560653(WS.10).aspx)|  
  
|||  
|-|-|  
|問題|建立新 AD DS 樹系一律會顯示警告 DNS|  
|症狀|在建立新的 AD DS 森林和建立的 DNS 區域新的網域控制站本身，您隨時收到警告訊息：<br /><br />程式碼-錯誤偵測 DNS 設定。 <br />這台電腦所使用的 DNS 伺服器的無回應中逾時長的時間間隔。<br />(錯誤碼 0x000005B4」ERROR_TIMEOUT」)|  
|解析度和筆記|略過。 以方便您想要指向現有的 DNS 伺服器及區域，為刻意根網域中的新的樹系的第一個網域控制站在這個警告。|  
  
|||  
|-|-|  
|問題|Windows PowerShell-引數傳回正確 DNS 伺服器的資訊|  
|症狀|如果您使用**-**時設定的網域控制站隱含或明確引數**-installdns: $true**，導致輸出所示：<br /><br />程式碼-」DNS 伺服器：否]|  
|解析度和筆記|略過。 DNS 已安裝的並設定正確。|  
  
|||  
|-|-|  
|問題|升級後，登入失敗，並「儲存空間不足是可用於處理此命令」|  
|症狀|新的網域控制站升級，然後先登出，嘗試互動方式登入之後，您收到錯誤訊息：<br /><br />程式碼的儲存空間不足是可用於處理此命令|  
|解析度和筆記|不網域控制站需要重新開機之後，因為發生錯誤升級，或是指定 ADDSDeployment Windows PowerShell 引數**-norebootoncompletion**。 重新開機網域控制站。|  
  
|||  
|-|-|  
|問題|[下一步] 按鈕並不適用於網域控制站選項] 頁面|  
|症狀|即使您已經設定密碼，**下一步**按鈕**網域控制站選項**頁面在伺服器管理員中不提供。 中列出的任何網站**網站名稱**功能表。|  
|解析度和筆記|您有多個 AD DS 網站和至少一個遺失子網路。此未來網域控制站屬於個子之一。 您必須手動選取子網路，從下拉式功能表名稱網站。 您也應該檢視使用 DSSITE.MSC] 或 [使用下列 Windows PowerShell 命令尋找所有網站遺失子網路：<br /><br />程式碼-取得-adreplicationsite-篩選 *-屬性子網路和 #124;where-object {！$_.subnets-eq」\ *"} 和 #124;格式化表格名稱|  
  
|||  
|-|-|  
|問題|訊息」的服務不會開始」的升級或降級失敗|  
|症狀|如果您嘗試升級、降級，或複製網域控制站您收到錯誤訊息：<br /><br />-的程式碼服務無法開始，因為它是停用或它就不讓的裝置相關的「(0x80070422)<br /><br />這個錯誤可能是互動，事件，或寫入 dcpromoui.log 或 dcpromo.log 等登入|  
|解析度和筆記|停用 DS 角色伺服器服務 (DsRoleSvc)。 根據預設，這項服務 AD DS 角色安裝期間安裝並手動開始輸入設定。 不要停用此服務。 將其設定為 [手動，並允許開始和停止它視 DS 角色作業。 此行為是設計。|  
  
|||  
|-|-|  
|問題|伺服器管理員仍會警告您需要提升俠|  
|症狀|如果您使用取代的 dcpromo.exe//unattend 網域控制站升級或就地現有 Windows Server 2008 R2 網域控制站升級到 Windows Server 2012，伺服器管理員仍會顯示部署後組態工作**這個網域控制站伺服器升級**。|  
|解析度和筆記|按一下部署後警告連結，並訊息會消失的好。 此行為是外觀與預期。|  
  
|||  
|-|-|  
|問題|伺服器管理員部署指令碼遺失角色安裝|  
|症狀|如果您使用伺服器管理員網域控制站升級，並儲存部署 Windows PowerShell 指令碼，它不包含的角色安裝 cmdlet 和引數 (windowsfeature 安裝-ad 網域服務-includemanagementtools 的名稱)。 的角色，而不設定 DC。|  
|解析度和筆記|手動將該 cmdlet 和引數任何指令碼。 此行為和所設計。|  
  
|||  
|-|-|  
|問題|伺服器管理員部署指令碼無法命名 PS1|  
|症狀|如果您使用伺服器管理員網域控制站升級，並儲存部署 Windows PowerShell 指令碼，隨機暫時名稱，而不是 PS1 檔案命名檔案。|  
|解析度和筆記|手動重新命名檔案。 此行為和所設計。|  
  
|問題|帶領//unattend 可支援功能層級|  
|-|-|  
|症狀|如果您升級網域控制站帶領//unattend 使用下列回應檔案範例：<br /><br />程式碼-<br /><br />[DCInstall]<br />NewDomain = 森林<br /><br />ReplicaOrNewDomain = 網域<br /><br />NewDomainDNSName = corp.contoso.com<br /><br />SafeModeAdminPassword =Safepassword@6<br /><br />DomainNetbiosName = corp<br /><br />DNSOnNetwork = [是]<br /><br />AutoConfigDNS = [是]<br /><br />RebootOnSuccess = NoAndNoPromptEither<br /><br />RebootOnCompletion = 否]<br /><br />*DomainLevel = 0*<br /><br />*ForestLevel = 0*<br /><br />促銷失敗，錯誤下列 dcpromoui.log:<br /><br />程式碼-出現 EA4.5B8 0089 13:31:50.783 Enter CArgumentsSpec::ValidateArgument DomainLevel<br /><br />出現 EA4.5B8 008A 13:31:50.783 DomainLevel 的值為 0<br /><br />出現 EA4.5B8 008B 13:31:50.783 結束程式碼是 77<br /><br />出現 EA4.5B8 008 C 13:31:50.783 指定引數不正確。<br /><br />出現 EA4.5B8 008 D 13:31:50.783 關閉登入<br /><br />出現 EA4.5B8 0032 13:31:50.830 結束程式碼是 77<br /><br />層級 0 是 Windows 2000，不支援 Windows Server 2012 中。|  
|解析度和筆記|不要使用已取代的帶領//unattend 並了解，它可以讓您設定不正確的更新失敗。 此行為和所設計。|  

|問題|建立 NTDS 設定物件，以「無回應」的升級永遠不會完成|  
|-|-|  
|症狀|如果您將升級俠或 RODC 複本，在升級到達」NTDS 建立設定物件」和一律不會繼續或完成。 登停止也更新。|  
|解析度和筆記|這是已知的問題，造成提供的認證建本機系統管理員符合入管理員密碼。 這造成失敗下並不是錯誤，而不斷等待（半進度）核心設定引擎。 這會如預期般-雖然非預期的行為。<br /><br />若要修正伺服器：<br /><br />1.重新開機。<br /><br />1.在 [廣告、delete 伺服器的成員電腦 account（不還會俠 account）<br /><br />1.在該 server 強制 disjoin 它的網域<br /><br />1.在 [伺服器，請移除 AD DS 角色。<br /><br />1.重新開機<br /><br />1.重新加入 AD DS 角色與 reattempt 升級，確保您總是提供***domain\admin***格式化俠促銷並不只是建本機系統管理員 account 認證|  
