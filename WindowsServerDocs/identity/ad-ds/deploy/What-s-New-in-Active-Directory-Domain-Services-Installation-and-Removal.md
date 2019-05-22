---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: Active Directory Domain Services 安裝和移除的新功能
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 66455a9ec4eb8a6ff6bfcfa387aeb59acb3ddcc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821349"
---
# <a name="whats-new-in-active-directory-domain-services-installation-and-removal"></a>Active Directory Domain Services 安裝和移除的新功能

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

更簡單和更舊版的 Windows Server active Directory 網域服務 (AD DS) 部署 Windows Server 2012 中。 AD DS 安裝程序現在建置於 Windows PowerShell 上，而且與 [伺服器管理員] 整合。 將網域控制站引入現有 Active Directory 環境所需執行的步驟也減少了。 這樣讓建立新 Active Directory 環境的程序變得更簡單且更有效率。 新的 AD DS 部署程序將可能阻止安裝的錯誤的機率降到最低。  
  
此外，您還可以同時在多部伺服器上安裝 AD DS 伺服器角色二進位檔 (它是 AD DS 伺服器角色)。 您也可以從遠端在個別伺服器上執行 AD DS 安裝精靈。 這些增強功能提供更具彈性的部署執行 Windows Server 2012，特別是針對大規模的全域部署許多網域控制站要部署到不同區域中的辦公室的網域控制站。  
  
AD DS 安裝包括下列功能：  
  
- **Adprep.exe 整合到 AD DS 安裝程序。** 準備現有 Active Directory 所需的繁複步驟，像是需要使用各種不同的認證、複製 Adprep.exe 檔案或登入特定網域控制站等，現在都已經簡化或可以自動執行了。 這樣就縮短了安裝 AD DS 所需的時間，並減少了可能阻止網域控制站升級的錯誤的機率。  

   對於在安裝新網域控制站之前最好預先執行 adprep.exe 命令的環境而言，您仍然可以將 adprep.exe 命令與 AD DS 安裝分開執行。 Windows Server 2012 版本的 adprep.exe 執行遠端電腦上，所以您可以從執行 Windows Server 2008 或更新版本的 64 位元版本的伺服器執行所有必要的命令。  

- **新的 AD DS 安裝建置於 Windows PowerShell 上，並可從遠端叫用。** 新的 AD DS 安裝與 [伺服器管理員] 整合，所以您可以使用與安裝其他伺服器角色所用的相同介面來安裝 AD DS。 對於 Windows PowerShell 使用者，AD DS 部署 Cmdlet 提供了更多的功能及彈性。 命令列與 GUI 的安裝選項功能是對等的。  
- **新的 AD DS 安裝包含先決條件驗證。** 任何可能出現的錯誤會在安裝開始前就先發現。 您可以在錯誤狀況發生之前就進行修正，而不用擔心會出現部分完整升級的錯誤。 例如，如果必須執行 adprep /domainprep，安裝精靈會確認使用者的權限足以執行操作。  
- **根據最常用的升級選項順序來分組設定頁面，以更少的精靈頁面來包含相關的選項。** 這可為安裝選項提供較佳的內容。  
- **您可以匯出包含圖形化安裝期間指定之所有選項的 Windows PowerShell 指令碼。** 在安裝或移除的最後，您可以將設定匯出到 Windows PowerShell 指令碼，以便用於自動化相同的操作。  
- **在重新開機之前只會執行關鍵性複寫。** 新的參數允許在重新開機前複寫非關鍵性資料。 如需詳細資訊，請參閱 [ADDSDeployment cmdlet arguments](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)。  

## <a name="BKMK_ADConfigurationWizard"></a>Active Directory 網域服務設定精靈

從 Windows Server 2012 開始，Active Directory 網域服務設定精靈可以取代舊版 Active Directory 網域服務安裝精靈做為指定的設定，當您安裝的網域控制站的使用者介面 (UI) 選項。 [Active Directory 網域服務設定精靈] 在 [新增角色精靈] 完成後開始。  

> [!WARNING]  
> 舊版 Active Directory 網域服務安裝精靈 (dcpromo.exe) 從 Windows Server 2012 開始已被取代。  

在 [安裝 「 Active Directory 網域服務 」&#40;技術等級 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)，UI 程序示範如何啟動 [新增角色精靈] 安裝 AD DS 伺服器角色二進位檔，然後執行 [Active Directory 網域服務組態精靈以完成安裝網域控制站。 Windows PowerShell 範例示範如何使用 AD DS 部署 Cmdlet 完成這兩個程序的步驟。  
  
## <a name="BKMK_NewAdprep"></a>Adprep.exe 整合

從 Windows Server 2012 開始，沒有只有一個版本的 Adprep.exe (沒有 32 位元版本 adprep32.exe)。 視需要安裝至現有的 Active Directory 網域或樹系中執行 Windows Server 2012 的網域控制站時自動執行 Adprep 命令。  
  
雖然 adprep 作業會自動執行，但是您可以個別執行 Adprep.exe。 例如，如果安裝 AD DS 的使用者不是 Enterprise Admins 群組的成員 (必須是這個群組的成員才能執行 Adprep /forestprep)，那麼可能需要另外執行命令。 但是，您只需要執行 adprep.exe 如果您計劃就地升級第一個 Windows Server 2012 網域控制站 （也就是說，您計劃就地升級執行 Windows Server 2012 的網域控制站的作業系統）。  
  
Adprep.exe 位於 Windows Server 2012 安裝光碟的 \support\adprep 資料夾中。Windows Server 2012 版本是 adprep 的能夠從遠端執行。  
  
Windows Server 2012 版本的 adprep.exe 可以在執行 64 位元版本的 Windows Server 2008 或更新版本的任何伺服器上執行。 伺服器需要建立網路連線到您想要新增網域控制站的樹系架構主機和網域基礎結構主機。 如果這些角色的任一個是裝載於執行 Windows Server 2003 的伺服器上，則 adprep 必須從遠端執行。 執行 adprep 的伺服器不需要是網域控制站。 它可以是加入網域或屬於工作群組的伺服器。  

> [!NOTE]  
> 如果您嘗試在執行 Windows Server 2003 的伺服器上執行 Windows Server 2012 版本的 adprep.exe，則會出現下列錯誤：  
>   
> Adprep.exe 不是有效的 Win32 應用程式。  

![新功能](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)  

如需解決 Adprep.exe 傳回的其他錯誤的相關資訊，請參閱 [Known issues](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues)。  

### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>檢查 Windows Server 2003 操作主機角色的群組成員資格

對於每個命令 (/forestprep、/domainprep 或 /rodcprep)，Adprep 會執行群組成員資格檢查，判斷指定的認證是否代表特定群組中的帳戶。 為了執行這項檢查，Adprep 會連絡操作主機角色擁有者。 如果操作主機是執行 Windows Server 2003，那麼在您執行 Adprep.exe 的時候，必須指定 /user 與 /userdomain 命令列參數，以確保在所有情況下都會執行群組成員資格檢查。  
  
/User 與 /userdomain 是 Windows Server 2012 中 Adprep.exe 的新參數。 這些參數分別指定執行 adprep 命令之使用者的使用者帳戶名稱及使用者網域。 Adprep.exe 命令列公用程式會阻止指定 /userdomain 和 /user 的其中一個，但略過另一個。  
  
不過，Adprep 操作也可以做為使用 Windows PowerShell 或 [伺服器管理員] 進行 AD DS 安裝的一部分來執行。 這些經驗與 adprep.exe 共用相同的基礎實作 (adprep.dll)。 Windows PowerShell 與 [伺服器管理員] 經驗有自己的個別認證輸入，其需求與 adprep.exe 不完全相同。 使用 Windows PowerShell 或 [伺服器管理員]，就可以將 /user (但不是 /userdomain) 的值傳送給 adprep.dll。 如果指定了 /user，但未指定 /userdomain，本機電腦的網域用來執行檢查。 如果電腦沒有加入網域，就無法檢查群組成員資格。  
  
當無法檢查群組成員資格的時候，Adprep 會在 adprep 記錄檔中顯示警告訊息，並繼續進行：  

```
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.  
```

如果執行 Adprep.exe 但沒有指定 /user 與 /userdomain 參數，而且操作主機是執行 Windows Server 2003，那麼 Adprep.exe 會連線目前登入使用者之網域中的網域控制站。 如果目前登入的使用者不是網域帳戶，Adprep.exe 就無法執行群組成員資格檢查。 如果使用了智慧卡認證，即使同時指定了 /user 與 /userdomain，Adprep.exe 還是不能執行群組成員資格檢查。  
  
如果 Adprep 成功完成，不需要其他的動作。 如果執行期間出現存取錯誤而導致 Adprep 失敗，請提供具有正確成員資格的帳戶。 如需詳細資訊，請參閱[執行 Adprep.exe 及安裝 Active Directory 網域服務的認證需求](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。  
  
### <a name="syntax-for-adprep-in-windows-server-2012"></a>Windows Server 2012 中 Adprep 的語法

使用下列語法將 adprep 與 AD DS 安裝分開執行：  

```
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *  
```

使用命令中的 /logdsid 以便產生更多詳細的記錄。 adprep.log 位於 %windir%\System32\Debug\Adprep\Logs 中。  

### <a name="running-adprep-using-smartcard"></a>使用智慧卡執行 adprep

Windows Server 2012 版本的 adprep.exe 可讓您可以使用智慧卡當作認證，但沒有指定的智慧卡認證，透過命令列的不容易。 有一個方式是透過 PowerShell Cmdlet Get-Credential 獲取智慧卡認證。 然後使用傳回的 PSCredential 物件的使用者名稱 (顯示為 `@@...`)。 密碼是智慧卡的 PIN。  

如果有指定 /user，Adprep.exe 就需要 /userdomain。 對於智慧卡認證，/userdomain 應該是智慧卡代表的基礎使用者帳戶的網域。  

### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>Adprep /domainprep /gpprep 命令不會自動執行。

adprep /domainprep /gpprep 命令不會做為 AD DS 安裝的一部分來執行。 這個命令會設定原則結果組 (RSOP) 計劃模式功能所需的權限。 如需這個命令的詳細資訊，請參閱 [Microsoft 知識庫文章 324392](https://support.microsoft.com/kb/324392)。 如果命令需要在 Active Directory 網域中執行，您可以將它與 AD DS 安裝分開執行。 如果準備部署執行 Windows Server 2003 SP1 或更新版本的網域控制站時已經執行了命令，就不需要再次執行命令。  

您可以安全地新增至現有的網域，而不需要執行 adprep /domainprep /gpprep 就執行 Windows Server 2012 網域控制站，但是 RSOP 計劃模式無法正確運作。  

## <a name="BKMK_PrereqCheck"></a>AD DS 安裝先決條件驗證

AD DS 安裝精靈會在安裝開始之前，先檢查是否符合下列先決條件。 這讓您有機會先行修正可能阻止安裝的問題。  
  
例如，Adprep 相關的先決條件包括：  

- Adprep 認證驗證：如果必須執行 adprep，安裝精靈會確認使用者的權限足以執行必要的 Adprep 操作。  
- 架構主機可用性檢查：如果安裝精靈判斷需要執行 adprep /forestprep，它會確認架構主機已經連線，否則會失敗。  
- 基礎結構主機可用性檢查：如果安裝精靈判斷需要執行 adprep /domainprep，它會確認基礎結構主機已經連線，否則會失敗。

從舊版 [Active Directory 安裝精靈]\(dcpromo.exe) 開始執行的其他先決條件檢查包括：  

- 樹系名稱驗證：確定樹系名稱是有效的，而且目前不存在。  
- NetBIOS 名稱驗證：檢查提供的 NetBIOS 名稱是有效的，而且與現有名稱沒有衝突。  
- 元件路徑驗證：確認 Active Directory 資料庫、記錄檔以及 SYSVOL 的路徑是有效的，而且有足夠的可用磁碟空間儲存它們。  
- 子網域名稱驗證：確定父系及新增的子網域名稱是有效的，而且它們與現有網域沒有衝突。  
- 樹狀目錄網域名稱驗證：確定指定的樹狀目錄名稱是有效的，而且目前不存在。  

## <a name="BKMK_SystemReqs"></a>系統需求

適用於 Windows Server 2012 的系統需求不會從 Windows Server 2008 R2 的變更。 如需詳細資訊，請參閱 < [Windows Server 2008 R2 含 SP1 系統需求](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx)(https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx)。  

某些功能可能會有其他的需求。 例如，虛擬網域控制站複製功能需要 PDC 模擬器執行 Windows Server 2012 和安裝了 HYPER-V 角色且執行 Windows Server 2012 的電腦。  

## <a name="BKMK_KnownIssues"></a>已知的問題

本節列出一些會影響 Windows Server 2012 中的 AD DS 安裝的已知問題。 如需其他已知問題，請參閱[疑難排解網域控制站部署](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md)。  

- 當您從遠端執行 adprep /forestprep 時，如果 Windows 防火牆封鎖 WMI 存取架構主機，則下列錯誤會被記錄到 %systemroot%\system32\debug\adprep 中的 adprep 記錄內：  

   ```
   Adprep encountered a Win32 error.
   Error code: 0x6ba Error message: The RPC server is unavailable.  
   ```

   在這種情況下，可以在架構主機上直接執行 adprep /forestprep，或是執行下列其中一個命令，允許 WMI 流量通過 Windows 防火牆，就可以解決這個錯誤。

   對於 Windows Server 2008 或更新版本：

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes
   ```

   對於 Windows Server 2003：

   ```
   netsh firewall set service RemoteAdmin enable
   ```

   adprep 完成之後，您可以執行下列任一命令，再次封鎖 WMI 流量：

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no
   ```

   ```
   netsh firewall set service remoteadmin disable
   ```

- 您可以按下 Ctrl + C，取消 Install-ADDSForest Cmdlet。 這個取消會停止安裝，也會還原之前對伺服器狀態所做的任何變更。 但是發出取消命令之後，控制權不會交回給 Windows PowerShell，而且 Cmdlet 會無限期停滯。  
- **如果目標伺服器未加入網域，再進行安裝，安裝其他網域控制站使用智慧卡認證將會失敗。**  

   這個情況中傳回的錯誤訊息如下：  

   無法連線複寫來源網域控制站 *來源網域控制站名稱*。 (例外：登入失敗：未知的使用者名稱或密碼錯誤)  

   如果將目標伺服器加入網域，然後使用智慧卡執行安裝，則安裝會成功。  
  
- **ADDSDeployment 模組不會在 32 位元程序下執行。** 如果您想要自動化部署和設定 Windows Server 2012 使用包含了 ADDSDeployment cmdlet 以及不支援原生的 64 位元處理序的任何其他 cmdlet 的指令碼，指令碼會失敗並出現錯誤，指出 ADDSDeployment找不到 cmdlet。  

   在這個情況下，您需要將 ADDSDeployment Cmdlet 與不支援原生 64 位元程序的 Cmdlet 分開執行。  

- Windows Server 2012，稱為復原檔案系統中沒有新的檔案系統。 請不要將 Active Directory 資料庫、記錄檔或 SYSVOL 儲存到使用復原檔案系統 (ReFS) 格式化的資料磁碟區。 如需 ReFS 的詳細資訊，請參閱[針對 Windows 建置新一代的檔案系統：ReFS](http://blogs.msdn.com/b/b8/archive/2012/01/16/building-the-next-generation-file-system-for-windows-refs.aspx)。  
- 在 伺服器管理員，在 Server Core 安裝上執行 AD DS 或其他伺服器角色，並已升級至 Windows Server 2012，伺服器的伺服器角色都具有紅色狀態，即使如預期收集事件和狀態。 執行初步發行版，也可能影響 Windows Server 2012 的 Server Core 安裝的伺服器。  

### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>如果錯誤阻止關鍵性複寫，Active Directory 網域服務安裝會停滯

如果 AD DS 安裝在關鍵性複寫階段碰到錯誤，安裝會無限期停滯。 例如，如果是網路錯誤防止關鍵性複寫完成，安裝將不會繼續。  
  
如果您使用 [伺服器管理員] 進行安裝，可能會看到安裝進度頁面維持開啟，但是螢幕上沒有報告錯誤，而且 15 分鐘過去了進度還是沒有任何變化。 如果您是使用 Windows PowerShell，則 Windows PowerShell 視窗中顯示的進度超過 15 分鐘還是沒有任何變化。  
  
如果發生這種問題，請檢查目標伺服器上 %systemroot%\debug 資料夾中的 dcpromo.log 檔案。 記錄檔通常會指出重複複寫錯誤。 這個問題的一些已知原因包括：  

- 網路問題，阻止要升級的目標伺服器與複寫來源網域控制站之間進行關鍵性複寫。  

   例如，dcpromo.log 會顯示：  

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

   因為安裝程序會無限期地重試關鍵性複寫，基礎網路問題解決之後，網域控制站安裝就可以繼續。 視需要使用像是 ipconfig、nslookup 及 netmon 等工具來調查網路問題。 確保您正在升級的網域控制站與在 AD DS 安裝期間所選取的複寫夥伴之間有連線存在。 此外，還請確定名稱解析可正常運作。  

   系統會在開始安裝之前，於先決條件檢查期間驗證網路連線與名稱解析的 AD DS 安裝需求。 但是，部分錯誤狀況可能會在先決條件驗證發生之後且在安裝完成之前出現，例如，如果複寫夥伴在安裝期間無法使用。  

- 在複本網域控制站安裝期間，會為安裝認證指定目標伺服器的本機 Administrator 帳戶，而且本機 Administrator 帳戶的密碼與網域管理員帳戶的密碼相符。 在此情況下，您可以完成安裝精靈，並開始安裝之前您遇到 「 拒絕存取 」 的失敗。  

   例如，dcpromo.log 會顯示：  

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

   如果錯誤是因為指定本機系統管理員帳戶和密碼而產生，為了進行復原，您需要重新安裝作業系統、針對無法完成安裝的網域控制站帳戶 [執行中繼資料清理](https://technet.microsoft.com/library/cc816907(WS.10).aspx) ，然後使用網域管理員認證重試 AD DS 安裝。 重新啟動伺服器將不會更正這個錯誤狀況，因為伺服器將指出已安裝 AD DS，即使安裝並未成功完成也一樣。  

### <a name="BKMK_nonnormalDNSNameWarning"></a>指定非標準化的 DNS 名稱時，會發出警告 active Directory 網域服務設定精靈

如果您建立新的網域或樹系，且指定包含非標準化的國際化字元的 DNS 網域名稱，那麼 [Active Directory 網域服務設定精靈] 會顯示警告，說明名稱的 DNS 查詢可能會失敗。 雖然 DNS 網域名稱是在 [部署設定] 頁面上指定的，但是警告是在精靈稍後的 [先決條件檢查] 頁面上出現。  

如果使用未標準化的名稱，例如 füßball.com 或 'ΣΤ'.com 指定 DNS 網域名稱 (正規化版本： füssball.com 和 β)，嘗試使用 WinHTTP 存取它的用戶端應用程式會正規化再呼叫 Api 的名稱解析的名稱。 如果使用者在某些對話方塊上，輸入"'ΣΤ'.com"，因為"β 」，但是沒有 DNS 伺服器將會比"'ΣΤ'.com"的資源記錄會傳送 DNS 查詢。 使用者將無法解析名稱。  

下列範例說明使用非標準化的 IDN 名稱時可能發生的其中一個問題：  

1. 建立並註冊 dns 伺服器上使用非標準化的名稱為網域： füßball.com  
2. 機器"nps 「 已加入網域，並取得註冊其名稱： nps.füßball.com  
3. 用戶端應用程式嘗試連線到伺服器 nps.füßball.com  
4. 用戶端應用程式會嘗試解析呼叫 Api 的名稱解析名稱 nps.füßball.com。  
5. 正規化，因為名稱取得轉換成 nps.füssball.com 和 nps.füßball.com 為網路上進行查詢  
6. 用戶端應用程式，才能夠解析名稱，因為已註冊的名稱是 nps.füßball.com  

如果 [Active Directory 網域服務設定精靈] 中的 [先決條件檢查] 頁面上出現警告，請返回 [部署設定] 頁面，指定標準化的 DNS 網域名稱。 如果您使用 Windows PowerShell 安裝新的網域，請為 -DomainName 選項指定標準化的 DNS 名稱。  

如需 IDN 的詳細資訊，請參閱 [處理國際化網域名稱 (IDN)](https://msdn.microsoft.com/library/windows/desktop/dd318142(v=vs.85).aspx)。  
