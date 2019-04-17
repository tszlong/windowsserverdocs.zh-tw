---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: "功能與 #39; Active Directory 網域中的新 s 服務安裝並移除"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 814a6f1af9144db28e81163b21cb5da4b6e51b3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="what39s-new-in-active-directory-domain-services-installation-and-removal"></a>功能與 #39; Active Directory 網域中的新 s 服務安裝並移除

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題涵蓋：  
  
-   [Active Directory Domain Services 設定精靈](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_ADConfigurationWizard)  
  
-   [Adprep.exe 整合](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep)  
  
-   [AD DS 安裝必要驗證](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_PrereqCheck)  
  
-   [系統需求](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_SystemReqs)  
  
-   [已知的問題](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues)  
  
Windows Server 2012 中的 active Directory Domain Services (AD DS) deployment 是更簡單且更快，比舊版本的 Windows Server。 AD DS 安裝程序 Windows PowerShell 現在建置，並整合伺服器管理員使用。 減少的網域控制站引入現有的 Active Directory 環境所需的步驟。 這樣可建立新的 Active Directory 環境更簡單且更有效率的程序。 新 AD DS 部署程序最小化有否則封鎖安裝程式錯誤的機會。  
  
此外，您可以在此同時在多部伺服器上安裝 AD DS 伺服器角色二進位 （亦即伺服器角色 AD DS）。 您也可以從遠端執行 AD DS 安裝精靈中的個人伺服器上。 這些改良功能提供更大的網域控制站的大型、 全球部署許多網域控制站要部署至分公司不同地區執行 Windows Server 2012，尤其是用來部署彈性。  
  
AD DS 安裝包含下列功能：  
  
-   **Adprep.exe 整合到 AD DS 安裝程序。** 麻煩步驟準備現有 Active Directory，例如使用各種不同的認證，複製 Adprep.exe 檔案或特定的網域控制站在登入需要所需的所有的簡化或自動執行。 這會減少安裝 AD DS 所需的時間，並減少否則可能會封鎖網域控制站升級的錯誤。  
  
    針對環境中執行 adprep.exe 命令新的網域控制站安裝之前，最好使用時，您仍可以執行 adprep.exe 命令分開從 AD DS 安裝。 Windows Server 2012 版本的 adprep.exe 執行遠端電腦上，因此您可以從執行 64 位元版本的 Windows Server 2008，或更新版本的伺服器來執行所需的所有命令。  
  
-   **新 AD DS 安裝 Windows PowerShell 上建置，可以從遠端叫用。** 新的 AD DS 安裝整合的伺服器管理員中，讓您可以使用相同的介面安裝 AD DS，您可以使用當您安裝其他伺服器角色。 Windows PowerShell 使用者，AD DS 部署 cmdlet 提供更大的功能與彈性。 在命令列還有功能同位與 GUI 安裝選項。  
  
-   **新的 AD DS 安裝包含必要條件驗證。** 開始安裝之前，都會任何可能的錯誤。 它們發生而不需顧慮所造成的部分完成升級之前，您可以更正錯誤條件。 例如 adprep /domainprep 需要執行時，如果在安裝精靈會驗證使用者具有權執行作業。  
  
-   **設定頁面的群組順序鏡射最常見的升級選項以使用較少精靈] 頁面中相關的選項的需求。** 這提供更好的操作讓安裝選項。  
  
-   **您可以將匯出包含所有選項圖形安裝期間所指定的 Windows PowerShell 指令碼。** 在安裝或移除結尾，您可以使用的自動執行相同作業 Windows PowerShell 指令碼匯出設定。  
  
-   **僅限重要複寫早重新開機。** 新的切換控制允許複寫嚴重的資料，再重新開機。 如需詳細資訊，請查看[ADDSDeployment cmdlet 引數](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)。  
  
### <a name="BKMK_ADConfigurationWizard"></a>Active Directory Domain Services 設定精靈  
開始使用 Windows Server 2012，Active Directory Domain Services 組態精靈會取代為使用者介面 (UI) 選項指定設定當您安裝的網域控制站舊版 Active Directory Domain Services 安裝精靈。 Active Directory Domain Services 組態精靈會開始新增角色精靈完成後。  
  
> [!WARNING]  
> 開始使用 Windows Server 2012，被取代舊版 Active Directory Domain Services 安裝精靈 (dcpromo.exe)。  
  
在[安裝 「 Active Directory Domain Services 和 #40;層級 100 和 #41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)，顯示如何開始新增角色精靈安裝 AD DS 伺服器角色二進位檔 UI 程序，並執行 Active Directory Domain Services 設定精靈完成網域控制站安裝。 Windows PowerShell 範例顯示如何完成使用 AD DS 部署 cmdlet 這兩個步驟。  
  
### <a name="BKMK_NewAdprep"></a>Adprep.exe 整合  
開始使用 Windows Server 2012，還有 Adprep.exe 只有一個版本 (還有不 32 位元版本 adprep32.exe)。 視需要您安裝的網域控制站現有 Active Directory domain 或樹系執行 Windows Server 2012 時自動執行 Adprep 的命令。  
  
Adprep 作業就會自動執行，但您可以執行 Adprep.exe 另行購買。 例如，不群組成員的企業系統管理員 」，才能執行 Adprep /forestprep 所需的安裝 AD DS 的使用者如果然後您可能需要執行 「 命令另行購買。 但是，您只需要執行 adprep.exe 如果您打算就地升級第一個 Windows Server 2012 網域控制站 （亦即，您計畫就地升級執行 Windows Server 2012 」 的網域控制站的作業系統）。  
  
Adprep.exe 位於 \support\adprep 資料夾中的 Windows Server 2012 安裝光碟。Windows Server 2012 的 adprep 版本遠端執行的功能。  
  
Windows Server 2012 執行的版本 adprep.exe 可執行 64 位元版本的 Windows Server 2008，或更新版本的任何伺服器上。 伺服器必須網路連接到架構主機樹系與基礎結構主機您想要加入的網域控制站的網域。 如果在執行 Windows Server 2003 的伺服器上的角色是裝載，然後 adprep 必須執行遠端。 執行 adprep 的伺服器不需要為網域控制站。 它可以加入或工作群組中的網域。  
  
> [!NOTE]  
> 如果您嘗試執行 adprep.exe 新版 Windows Server 2012 上執行 Windows Server 2003 的伺服器，會顯示下列錯誤：  
>   
> Adprep.exe 不是有效的 Win32 應用程式。  
  
![新功能](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)  
  
如解析傳回 Adprep.exe 其他錯誤相關資訊，請查看[的已知問題](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues)。  
  
#### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>針對 Windows Server 2003 操作主機角色群組成員資格核取  
針對每個命令 (日 forestprep，/domainprep 或 /rodcprep)，Adprep 執行群組成員資格檢查以判斷是否指定的憑證表示 account 特定群組中的。 若要執行此核取，Adprep 連絡人主角擁有者作業。 操作主機執行的 Windows Server 2003，如果您需要指定 /user 與 /userdomain 命令列參數，如果您執行 Adprep.exe 以確保所有案例中執行群組成員資格檢查。  
  
/User 和 /userdomain 是 Adprep.exe Windows Server 2012 中的新參數。 這些參數指定的 account 的使用者名稱和使用者網域的使用者身分執行 adprep 的命令。 命令列 Adprep.exe 公用程式會封鎖指定 /userdomain 和 /user 但省略另一個。  
  
不過，Adprep 作業也可以使用 Windows PowerShell 或伺服器管理員 AD DS 安裝的一部分執行。 這些體驗共用相同 adprep.exe 為基礎實作 (adprep.dll)。 Windows PowerShell 和伺服器管理員體驗有輸入時，這會不加相同的需求為 adprep.exe 他們不同的認證。 使用 Windows PowerShell 或伺服器管理員 」，則可能傳送 adprep.dll /user，但不 /userdomain 值。 如果指定 /user，但未指定 /userdomain，本機電腦的網域用來執行檢查。 如果您未加入網域的電腦，就無法檢查群組成員資格。  
  
當群組成員資格無法檢查時、 Adprep adprep 登入檔案中會顯示一則警告訊息，並持續：  
  
```  
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.  
```  
  
如果您不需要指定 /user 和 /userdomain 參數執行 Adprep.exe 操作主機執行 Windows Server 2003，Adprep.exe 連絡人網域控制站的目前登入的使用者網域中。 如果不核對目前登入的使用者，Adprep.exe 無法執行群組成員資格檢查。 Adprep.exe 也無法執行群組成員資格檢查是否使用智慧卡上的憑證，即使您指定 /user 和 /userdomain。  
  
如果 Adprep 成功完成，就不需要執行任何動作。 如果 Adprep 失敗的錯誤存取執行期間，帳號提供正確的資格。 如需詳細資訊，請查看[認證需求執行 Adprep.exe 並安裝 Active Directory Domain Services](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。  
  
#### <a name="syntax-for-adprep-in-windows-server-2012"></a>Windows Server 2012 中 Adprep 語法  
使用下列語法 adprep 執行分開 AD DS 安裝：  
  
```  
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *  
```  
  
使用命令中 /logdsid 產生詳細登入。 準備這個網域位於 %windir%\System32\Debug\Adprep\Logs。  
  
#### <a name="running-adprep-using-smartcard"></a>執行 adprep 使用智慧卡  
Adprep.exe 的 Windows Server 2012 版本的運作方式使用智慧卡認證，但不容易指定智慧卡憑證透過命令列。 一個方法是取得透過 PowerShell cmdlet 取得認證智慧卡認證。 使用的使用者名稱傳回 PSCredential 物件，會顯示為`@@...`。 密碼不智慧卡的 pin 碼。  
  
如果指定 /user Adprep.exe 會需要 /userdomain。 智慧卡認證，/userdomain 應該由智慧卡基礎帳號的網域。  
  
#### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>不會自動執行 Adprep /domainprep /gpprep 命令  
不是執行 adprep /domainprep /gpprep 命令 AD DS 安裝的一部分。 這個命令設定所需的結果設定的原則 (RSOP) 權限計劃模式功能。 如需有關這個命令時，請查看[Microsoft 知識庫文章 324392](https://support.microsoft.com/kb/324392)。 如果需要執行 Active Directory 網域中的命令，可以從 AD DS 安裝分開執行它。 如果您已經準備部署執行 Windows Server 2003 SP1 網域控制站的執行命令或更新版本，命令不需要再執行一次。  
  
您可以放心地加入網域控制站的現有執行 adprep /domainprep /gpprep，而網域執行 Windows Server 2012，但 RSOP 計劃模式無法正確運作。  
  
### <a name="BKMK_PrereqCheck"></a>AD DS 安裝必要驗證  
AD DS 安裝精靈會檢查開始安裝之前，符合下列必要條件。 這提供您機會修正這些問題可以可能會封鎖安裝程式。  
  
例如，Adprep 相關必要條件包括：  
  
-   Adprep 認證驗證： 如果 adprep 需要執行，安裝精靈中驗證使用者有權來執行所需的 Adprep 作業。  
  
-   架構主要可用性檢查： 如果安裝精靈判斷該 adprep /forestprep 必須執行時，它會確認架構主機是 online 否則失敗。  
  
-   基礎結構主要可用性核取： 如果安裝精靈判斷該 adprep /domainprep 必須執行時，它會確認基礎結構主機是 online 否則失敗。  
  
其他必要條件檢查發展從舊版 Active Directory 安裝精靈 (dcpromo.exe)，包括：  
  
-   樹系名稱驗證： 確保樹系名稱無效的而且目前不存在。  
  
-   NetBIOS 命名驗證： 檢查有效 NetBIOS 名稱，且不會衝突隨附現有的名稱。  
  
-   元件路徑驗證︰ 有效 Active Directory 資料庫、 登，和 SYSVOL 路徑為何，還有磁碟空間不足，無法為他們提供確認。  
  
-   子女的網域名稱驗證： 確保是有效的父系和新的子女的網域名稱和，它們不會衝突現有的網域。  
  
-   樹狀網域名稱驗證： 確保指定的樹名稱有效，而該它目前不存在。  
  
## <a name="BKMK_SystemReqs"></a>系統需求  
不會變更從 Windows Server 2008 R2 的 Windows Server 2012 系統需求。 如需詳細資訊，請查看[Windows Server 2008 R2 SP1 的系統需求與](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx)(https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx)。  
  
部分功能可能的額外需求。 例如，virtual 網域控制站複製功能需要，肯定執行 Windows Server 2012 和已安裝於 HYPER-V 的角色執行 Windows Server 2012 的電腦。  
  
## <a name="BKMK_KnownIssues"></a>已知的問題  
本節列出的已知問題影響 AD DS 安裝 Windows Server 2012 中的部分。 其他的已知問題，請查看[進行疑難排解的網域控制站部署](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md)。  
  
-   WMI 存取架構主機封鎖 Windows 防火牆 adprep /forestprep 遠端執行時，如果下列錯誤被登入，%systemroot%\system32\debug\adprep adprep 登入中：  
  
    ```  
    Adprep encountered a Win32 error.   
    Error code: 0x6ba Error message: The RPC server is unavailable.  
    ```  
  
    若是如此，您可以在錯誤由任一執行 adprep /forestprep 直接架構主機上，或您可以執行下列命令，讓 Windows 防火牆 WMI 流量的其中一個。  
  
    Windows Server 2008 或更新版本：  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes  
    ```  
  
    適用於 Windows Server 2003:  
  
    ```  
    netsh firewall set service RemoteAdmin enable  
    ```  
  
    Adprep 完成之後，您可以執行其中一項下列命令，再試一次封鎖 WMI 流量：  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no  
    ```  
  
    ```  
    netsh firewall set service remoteadmin disable  
    ```  
  
-   Ctrl + C 取消安裝-ADDSForest cmdlet，您可以輸入。 取消停止安裝，並會還原伺服器的狀態所做的任何變更。 但發出取消命令之後，控制項不回到 Windows PowerShell，並可無限期停止回應 cmdlet。  
  
-   **如果您未加入網域安裝之前目標伺服器使用智慧卡認證其他網域控制站安裝將會失敗。**  
  
    此時會傳回的錯誤訊息：  
  
    無法連接到複寫來源網域控制站*來源網域控制站名稱*。 (例外： Logonfailure： 不明的使用者名稱或錯誤密碼)  
  
    如果您加入網域的目標伺服器，並執行安裝使用智慧卡，完成安裝。  
  
-   **在 32 位元處理程序無法執行 ADDSDeployment 模組。** 如果您想自動化部署與 Windows Server 2012 使用指令碼，其中包含 ADDSDeployment cmdlet 與任何其他 cmdlet 不支援原生 64 位元處理程序，設定，指令碼可能會失敗，且會發生錯誤，指出 ADDSDeployment cmdlet 找不到。  
  
    若是如此，您需要執行 ADDSDeployment cmdlet 分開 cmdlet 不支援原生 64 位元處理程序。  
  
-   還有名復原檔案系統的 Windows Server 2012 中的新檔案系統。 不要使用復原檔案系統 (ReFS) 格式化是資料磁碟區上儲存 Active Directory 資料庫、 登入檔案或 SYSVOL。 如需 ReFS 的詳細資訊，請查看[上的下一代檔案系統建置適用於 Windows: ReFS](http://blogs.msdn.com/b/b8/archive/2012/01/16/building-the-next-generation-file-system-for-windows-refs.aspx)。  
  
-   在伺服器管理員中，執行 AD DS 或其他伺服器角色 Server Core 安裝上並已升級至 Windows Server 2012，伺服器伺服器角色才會出現的紅色狀態，即使事件和狀態，會收集如預期般運作。 執行 Windows Server 2012 可能也會受到影響預備版 Server Core 安裝的伺服器。  
  
### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>如果發生錯誤而無法重要複寫的 active Directory Domain Services 安裝無回應  
如果 AD DS 安裝重要複寫階段期間發生錯誤，安裝可無限期停止回應。 例如，如果網路錯誤避免重要複寫完成時，將不會繼續安裝。  
  
如果您使用伺服器管理員安裝，您可能會看到安裝進度頁面維持開放的但不錯誤報告，在畫面上，和進度可能不會變更大約 15 分鐘。 如果您使用 Windows PowerShell，超過 15 分鐘的時間不會變更 Windows PowerShell 視窗中顯示的進度。  
  
如果您遇到這個問題，請檢查 dcpromo.log 資料夾中的檔案 %systemroot%\debug 目標伺服器上。 登入檔案通常會指出重複複寫失敗。 是部分已知的原因，此問題：  
  
-   網路問題避免複寫來源網域控制站伺服器升級目標之間的重大複寫。  
  
    例如，會顯示 dcpromo.log:  
  
    ```  
  
      05/02/2012 14:16:46 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963  
      Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
    Process ID:   
    500  
    Reported error information:  
    Error value:   
    Could not find the domain controller for this domain. (1908)  
    directory service:   
    <domain>.com  
    Extensive error information:  
    Error value:   
    A security package specific error occurred. 1825  
    directory service:   
    <DC Name>  
    ```  
  
    安裝程序重試重要複寫不斷，因為網域控制站安裝進行如果解析網路問題。 調查使用工具，例如 ipconfig、 nslookup，以及 netmon 所需的網路問題。 確定連接存在之間升級您的網域控制站，並 AD DS 安裝時，選取複寫合作夥伴。 也會確保名稱解析正常運作。  
  
    開始安裝之前必要條件檢查會驗證網路連接名稱解析 AD DS 安裝需求。 但一些錯誤條件會的時間後必要條件驗證發生，在安裝完成之前，例如如果複寫合作夥伴無法在安裝期間發生。  
  
-   複本網域控制站在安裝期間，對於安裝認證指定的本機目標伺服器的和本機的密碼和網域管理員 account 的密碼。 若是如此，您可以完成安裝精靈中，並開始安裝之前，您會遇到 「 存取 「 失敗。  
  
    例如，會顯示 dcpromo.log:  
  
    ```  
  
    03/30/2012 11:36:51 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC2.contoso.com...  
    03/30/2012 11:36:51 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
    Process ID:   
    508  
    Reported error information:  
    Error value:   
    Access is denied. (5)  
    directory service:   
    DC2.contoso.com  
  
    ```  
  
    如果發生錯誤的原因是指定本機系統管理員 account 和密碼，以便復原您需要重新安裝作業系統，[執行的清除中繼資料](https://technet.microsoft.com/library/cc816907(WS.10).aspx)以完成安裝並再試一次使用網域管理員認證 AD DS 安裝失敗的網域控制站的帳號。 重新伺服器會不修正錯誤因為伺服器也會指出 AD DS，即使安裝未成功完成安裝。  
  
### <a name="BKMK_nonnormalDNSNameWarning"></a>Active Directory Domain Services 組態精靈會警告指定的時的非標準化 DNS 名稱  
如果您建立新的網域，或樹系和您指定 DNS 網域名稱包含不標準化國際化的字元，Active Directory Domain Services 組態精靈會顯示警告的 DNS 名稱查詢可以失敗。 指定 DNS 網域名稱中部署設定] 頁面，雖然必要條件檢查稍後精靈中的畫面上會出現的警告。  
  
如果您使用像是 füßball.com 或 'ΣΤ'.com 未標準化名稱指定 DNS 網域名稱 (標準化版本： füssball.com 和 βστα.com)，嘗試使用 WinHTTP 存取 client 應用程式將會之前通話名稱解析 Api 標準化名稱。 如果使用者在某些對話方塊 」 'ΣΤ'.com 」，如 「 βστα.com 」 並不 DNS 伺服器會使其符合資源記錄 「 'ΣΤ'.com 」 的使用將被傳送 DNS 查詢。 使用者無法解析名稱。  
  
下列範例解釋時使用的不標準化 IDN 名稱可能會發生的問題之一：  
  
1.  建立並登記 dns 伺服器上的網域使用非標準化名稱： füßball.com  
  
2.  已經加入網域的電腦 」 nps 」，並取得登記其名稱： nps.füßball.com  
  
3.  連接到伺服器 nps.füßball.com 嘗試 client 應用程式  
  
4.  Client 應用程式嘗試解析名稱 nps.füßball.com 撥打名稱解析 Api。  
  
5.  標準化，因為名稱取得轉換成 nps.füssball.com 和查詢透過 nps.füßball.com 為網路  
  
6.  Client 應用程式的名稱解析因為且已的名稱 nps.füßball.com 無法  
  
如果警告出現在 Active Directory Domain Services 組態精靈中的 [必要條件檢查] 頁面，返回 [部署設定] 頁面，然後指定標準化的 DNS 網域名稱。 如果您安裝新的網域使用 Windows PowerShell，指定標準化的 DNS 名稱的網域名稱] 選項。  
  
如需 Idn 的詳細資訊，請查看[處理國際化的網域名稱 (Idn)](https://msdn.microsoft.com/library/windows/desktop/dd318142(v=vs.85).aspx)。  
  

