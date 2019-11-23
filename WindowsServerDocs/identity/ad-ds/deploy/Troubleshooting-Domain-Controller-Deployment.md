---
ms.assetid: 5ab76733-804d-4f30-bee6-cb672ad5075a
title: 疑難排解網域控制站部署
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 88fc0e14569c395bd1479ead338d83bffc2fd72f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369620"
---
# <a name="troubleshooting-domain-controller-deployment"></a>疑難排解網域控制站部署

>適用於︰Windows Server 2016

本主題涵蓋網域控制站組態和部署的詳細疑難排解方法。  

## <a name="introduction-to-troubleshooting"></a>疑難排解簡介

![疑難排解](media/Troubleshooting-Domain-Controller-Deployment/adds_deploy_troubleshooting.png)  

## <a name="built-in-logs-for-troubleshooting"></a>用於疑難排解的內建記錄檔

內建記錄檔是疑難排解網域控制站升級和降級問題的最重要工具。 預設會啟用及設定所有的這些記錄檔以提供最詳盡的詳細資訊。  

|階段|記錄檔|  
|---------|-------|  
|伺服器管理員或 ADDSDeployment Windows PowerShell 作業|- %systemroot%\debug\dcpromoui.log<br /><br />-%systemroot%\debug\dcpromoui * .log|  
|網域控制站的安裝/升級|-%systemroot%\debug\dcpromo.log<br /><br />-%systemroot%\debug\dcpromo * .log<br /><br />-Event 檢視器 \windows 記錄 \ 系統<br /><br />-Event 檢視器 \windows logs\Application<br /><br />-事件 viewer\Applications 和服務 logs\Directory 服務<br /><br />-事件 viewer\Applications 和服務 logs\File 複寫服務<br /><br />-事件 viewer\Applications 和服務 logs\DFS 複寫|  
|樹系或網域升級|-%systemroot%\debug\adprep\\<datetime>\adprep.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\csv.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\dspecup.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\ldif.log *|  
|伺服器管理員 ADDSDeployment Windows PowerShell 部署引擎|-事件 viewer\Applications 和服務 logs\Microsoft\Windows\DirectoryServices-Deployment\Operational|  
|Windows 維護|-%systemroot%\Logs\CBS\\*<br /><br />- %systemroot%\servicing\sessions\sessions.xml<br /><br />- %systemroot%\winsxs\poqexec.log<br /><br />- %systemroot%\winsxs\pending.xml|  

### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>適用於疑難排解網域控制站設定的工具和命令

若要疑難排解記錄檔未說明的問題，請使用下列工具做為起點：  

-   Dcdiag.exe  

-   Repadmin.exe  

-   [AutoRuns.exe](https://technet.microsoft.com/sysinternals/bb963902.aspx)、工作管理員和 MSInfo32.exe  

-   [Network Monitor 3.4](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=4865) (或協力廠商網路擷取和分析工具)  

### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>疑難排解網域控制站設定的一般方法  

1.  因簡單的語法問題造成錯誤？  

    1.  您是否輸入錯誤或忘記提供 ADDSDeployment Windows PowerShell 引數？ 例如，如果使用 ADDSDeployment Windows PowerShell，您是否忘記新增名稱有效的必要引數 **-domainname**？  

    2.  請仔細檢查 Windows PowerShell 主控台輸出，以查看無法剖析所提供之命令列的確切原因。  

2.  是先決條件失敗的錯誤吗？  

    1.  許多之前顯示為嚴重升級結果的錯誤現在已可透過先決條件檢查程式來排除。  

    2.  請仔細檢查先決條件錯誤的文字，它們會提供必要指導方針以解決大部分問題，因為它們是受控制的狀況。  

3.  這是升級時發生錯誤而成為嚴重錯誤嗎？  

    1.  請仔細檢查結果：許多錯誤都有簡單說明，例如密碼錯誤、網路名稱解析錯誤，或重要網域控制站離線錯誤。  

    2.  請檢查輸出中顯示之錯誤的 Dcpromoui.log 和 dcpromo.log，然後從該兩個記錄檔往回查看此錯誤發生原因的指示。  

        1.  一律與工作中的範例記錄檔比較  

        2.  請只有在結果指示延伸架構或準備樹系或網域發生問題時，才檢查 ADPrep 記錄檔是否有錯誤。  

        3.  請只有在 Dcpromoui.log 缺少詳細資訊，或因為組態設定處理程序中有未處理的例外狀況而任意結束時，才檢查錯誤的 DirectoryServices-Deployment 事件記錄檔。  

    3.  請檢查其他組態問題指示器的目錄服務、系統及應用程式事件記錄檔。 通常網域控制站升級是只會影響所有分散式系統之其他網路設定錯誤的問題。  

    4.  請使用 dcdiag.exe 與 repadmin.exe 驗證整體樹系健康情況，以及指示可能阻止進一步網域控制站升級的細微設定錯誤。  

    5.  請使用 AutoRuns.exe、工作管理員或 MSinfo32.exe 來檢查電腦上可能會產生干擾的協力廠商軟體。  

        1.  移除協力廠商軟體 (不要只是停用軟體，因為這並不會阻止驅動程式載入)。  

    6.  在無法升級的電腦上和複寫夥伴網域控制站上安裝 NetMon 3.4，然後使用雙端網路擷取來分析升級處理程序。  

        1.  將此結果與您工作中的實驗室環境比較，以了解何謂正常升級情況及了解失敗的位置。  

        2.  此時，可能是樹系物件、 非預設安全性變更，或網路發生問題，而且這個新的網域控制站是 DNS、防火牆、主機入侵保護軟體設定錯誤，或其他外部因素的受害者。  

## <a name="troubleshooting-events-and-error-messages"></a>事件和錯誤訊息的疑難排解

網域控制站升級和降級一律會在作業結束時傳回代碼，而且和大部分程式不同，不會傳回零來表示作業成功。 若要查看網域控制站組態設定結束時的代碼，您有幾個選項可選：  

1. 使用伺服器管理員時，請在自動重新開機之前十秒內檢查升級結果。  

2. 使用 ADDSDeployment Windows PowerShell 時，請在自動重新開機之前十秒內檢查升級結果。 或者，在完成時選擇不要自動重新啟動。 您應該新增**格式清單**管線以方便閱讀輸出結果。 例如：  

   ```  
   Install-addsdomaincontroller <options> -norebootoncompletion:$true | format-list  

   ```  

   先決條件驗證和確認錯誤不會在重新開機時持續發生，因此在所有情況下都會看到那些錯誤。 例如：  

   ![疑難排解](media/Troubleshooting-Domain-Controller-Deployment/ADDS_PSPrereqError.png)  

3. 請在任何情況下同時檢查 dcpromo.log 和 dcpromoui.log。  

   > [!NOTE]  
   > 下面列出的部分錯誤已經因為較新版作業系統中的作業系統與網域控制站組態變更而不可能再發生。 新的 ADDSDeployment Windows PowerShell 程式碼也能防止某些錯誤，但 dcpromo.exe /unattend 並沒有此功能，這是必須將您目前所有的自動化作業從已過時之 DCPromo 切換成 ADDSDeployment Windows Powershell 的另一個原因。  

### <a name="promotion-and-demotion-success-codes"></a>升級與降級成功代碼

|錯誤碼|說明|注意|  
|--------------|---------------|--------|  
|1|順利結束|您仍然必須重新開機，這只是已移除自動重新啟動旗標的附註|  
|2|順利結束，需重新開機||  
|3|順利結束，但發生非重大失敗|通常會在傳回 DNS 委派警告時出現。 如果未設定 DNS 委派，請使用：<br /><br />-creatednsdelegation:$false|  
|4|順利結束，但發生非重大失敗，需重新開機|通常會在傳回 DNS 委派警告時出現。 如果未設定 DNS 委派，請使用：<br /><br />-creatednsdelegation:$false|  

### <a name="promotion-and-demotion-failure-codes"></a>促銷和降級失敗代碼

升級和降級會傳回下列失敗訊息代碼。 也可能是延伸的錯誤訊息，請務必仔細閱讀完整錯誤訊息，不要只注意數字部分。  


| 錯誤碼 |                                                           說明                                                            |                                                                                                                            建議的解決方式                                                                                                                            |
|------------|----------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     11     |                                          網域控制站已經在執行中                                          |                                                                                 請勿為相同目標電腦同時執行超過一個網域控制站升級執行個體                                                                                  |
|     12     |                                                    使用者必須是系統管理員                                                    |                                                                                        請以內建系統管理員群組成員的身份登入，並確定您是使用 UAC 來提升                                                                                        |
|     13     |                                               已安裝憑證授權單位                                               | 您無法將此網域控制站降級，因為它也是憑證授權單位。 請勿在未仔細清查 CA 的使用狀況之前將它移除。如果該 CA 正在簽發憑證，移除角色將會導致中斷。 不鼓勵在網域控制站上執行 CA |
|     14     |                                                    以安全開機模式執行                                                     |                                                                                                                      請以標準模式將伺服器開機                                                                                                                      |
|     15     |                                            正在變更角色或需要重新開機                                            |                                                                                             您必須先重新啟動伺服器 (因為之前組態變更) 再升級                                                                                              |
|     16     |                                                    在錯誤的平台上執行                                                     |                                                                                                                       *不可能收到此錯誤*                                                                                                                       |
|     17     |                                                      沒有 NTFS 5 磁碟機存在                                                      |                                                                            Windows Server 2012 中不可能發生此錯誤，因為至少必須將 %systemdrive% 格式化為 NTFS                                                                             |
|     18     |                                                    windir 中沒有足夠空間                                                    |                                                                                                        請使用 cleanmgr.exe 釋放 %systemdrive% 磁碟區的空間                                                                                                        |
|     19     |                                                名稱變更擱置。需要重新開機                                                 |                                                                                                                             請將伺服器重新開機                                                                                                                              |
|     20     |                                                 電腦名稱的語法無效                                                  |                                                                                                                   請將電腦重新命名為有效名稱                                                                                                                    |
|     21     |                             此網域控制站保有 FSMO 角色、是一個 GC，以及 (或) 是一部 DNS 伺服器                             |                                                                                                      使用 **-forceremoval**時新增 **-demoteoperationmasterrole** 。                                                                                                      |
|     22     |                                        必須安裝 TCP/IP，或 TCP/IP 未運作                                         |                                                                                                    請確認電腦中已設定、已繫結 TCP/IP，並在正確工作中                                                                                                     |
|     23     |                                             必須先設定 DNS 用戶端                                              |                                                                                                  請在新增網域控制站至網域時設定主要 DNS 伺服器                                                                                                  |
|     24     |                                  提供的認證無效或遺失必要項目                                   |                                                                                                               請確認您的使用者名稱和密碼是否正確                                                                                                                |
|     25     |                                 找不到指定網域的網域控制站                                  |                                                                                                                請驗證 DNS 用戶端設定、防火牆規則                                                                                                                |
|     26     |                                        無法從樹系讀取網域清單                                         |                                                                                                      請驗證 DNS 用戶端設定、 LDAP 功能、 防火牆規則                                                                                                      |
|     27     |                                                       遺失網域名稱                                                        |                                                                                                                請在升級或降級時指定網域                                                                                                                 |
|     28     |                                                         網域名稱無效                                                          |                                                                                                          請在升級時選擇不同且有效的 DNS 網域名稱                                                                                                          |
|     29     |                                                   父系網域不存在                                                   |                                                                                             請在建立新的子網域或樹狀目錄網域時驗證指定的父系網域                                                                                             |
|     30     |                                                       網域不在樹系中                                                       |                                                                                                                      請驗證提供的網域名稱                                                                                                                       |
|     31     |                                                   子網域已存在                                                    |                                                                                                                      請指定不同的網域名稱                                                                                                                       |
|     32     |                                                     NetBIOS 網域名稱無效                                                      |                                                                                                                    請指定有效的 NetBIOS 網域名稱                                                                                                                     |
|     33     |                                                 IFM 檔案的路徑無效                                                 |                                                                                                            請驗證您 [從媒體安裝] 資料夾的路徑                                                                                                             |
|     34     |                                                     IFM 資料庫無效                                                      |                                                          請使用此作業系統和角色 (相同作業系統版本、 相同類型網域控制站 - RODC 與 RWDC 比較) 之正確的「從媒體安裝」                                                          |
|     35     |                                                          遺失 SYSKEY                                                          |                                                                                             「從媒體安裝」已加密，因此您必須提供有效的 SYSKEY 才能使用它                                                                                              |
|     37     |                                          NTDS 資料庫或其記錄檔的路徑無效                                           |                                                                                          請將資料庫和記錄檔的路徑變更至固定的 NTFS 磁碟區、而非對應的磁碟機或 UNC 路徑                                                                                           |
|     38     |                                   磁碟區沒有足夠空間可供 NTDS 資料庫或記錄檔使用                                    |                                                                              請使用 cleanmgr.exe  釋放空間、新增更多磁碟空間，或將不必要的資料移到其他位置來手動清理空間                                                                              |
|     39     |                                                    SYSVOL 的路徑無效                                                    |                                                                                            請將 SYSVOL 資料夾的路徑變更至固定的 NTFS 磁碟區、而非對應的磁碟機或 UNC 路徑                                                                                             |
|     40     |                                                        無效的站台名稱                                                         |                                                                                                                      請提供存在的站台名稱                                                                                                                       |
|     41     |                                             必須指定安全模式的密碼                                             |                                                                                請提供 DSRM 帳戶的密碼，無論密碼原則的設定方式為何，密碼都不能空白                                                                                 |
|     42     |                                    安全模式密碼不符合準則 (僅限升級)                                    |                                                                                         請為 DSRM 帳戶提供符合密碼原則設定規則的密碼                                                                                          |
|     43     |                                      管理密碼不符合準則 (僅限降級)                                       |                                                                                  請為本機系統管理員帳戶提供符合密碼原則設定規則的密碼                                                                                  |
|     44     |                                           指定的樹系名稱無效                                           |                                                                                                                請指定有效的樹系根 DNS 網域名稱                                                                                                                 |
|     45     |                                         具有指定名稱的樹系已存在                                          |                                                                                                               請選擇其他樹系根 DNS 網域名稱                                                                                                               |
|     46     |                                            指定的樹狀目錄名稱無效                                            |                                                                                                                    請指定有效的樹狀目錄 DNS 網域名稱                                                                                                                    |
|     47     |                                          具有指定名稱的樹狀目錄已存在                                           |                                                                                                                  請選擇其他樹狀目錄 DNS 網域名稱                                                                                                                   |
|     48     |                                       樹狀目錄名稱與樹系結構不符                                       |                                                                                                                  請選擇其他樹狀目錄 DNS 網域名稱                                                                                                                   |
|     49     |                                               指定的網域不存在                                                |                                                                                                                       請驗證您輸入的網域名稱                                                                                                                        |
|     50     | 在降級期間，偵測到最後一個網域控制站 (即使它並不是)，或已指定最後一個網域控制站 (但它並不是) |        除非網域控制站確實是**網域中最後一個網域控制站**( **-lastdomaincontrollerindomain**)，否則請勿指定此項目。 如果這確實是最後一個網域控制站，而且有虛設網域控制站中繼資料，請使用 **-ignorelastdcindomainmismatch**來覆寫        |
|     51     |                                          此網域控制站上有應用程式磁碟分割存在                                          |                                                                                              請指定**移除應用程式磁碟分割**( **-removeapplicationpartitions**)                                                                                               |
|     52     |            遺失必要的命令列引數 (也就是命令列中必須指定回應檔案)             |                                                                                              *只有在已被取代的 dcpromo/unattend 中才看得到。請參閱舊版檔*                                                                                              |
|     53     |                               升級/降級失敗，必須重新開機以清理電腦                                |                                                                                                                    請檢查延伸錯誤和記錄檔                                                                                                                     |
|     54     |                                                  升級/降級失敗                                                   |                                                                                                                    請檢查延伸錯誤和記錄檔                                                                                                                     |
|     55     |                                         使用者已取消升級/降級                                          |                                                                                                                    請檢查延伸錯誤和記錄檔                                                                                                                     |
|     56     |                      使用者已取消升級/降級，必須重新開機以清理電腦                       |                                                                                                                    請檢查延伸錯誤和記錄檔                                                                                                                     |
|     58     |                                       必須在 RODC 升級期間指定站台名稱                                        |                                                                                           您必須指定 RODC 的站台，它不會像 RWDC 一樣自動偵測站台                                                                                           |
|     59     |                        此網域控制站在降級期間是其其中一個區域中的最後一部 DNS 伺服器                         |                                                                                    請指定這是**網域中最後一個 DNS 伺服器**或使用 **-ignorelastdnsserverfordomain**                                                                                     |
|     60     |         網域中必須有執行 Windows Server 2008 或更新版本的網域控制站存在才能升級 RODC          |                                                                                             請至少升級一個 Windows Server 2008 或更新版本的模型可寫入網域控制站                                                                                             |
|     61     |        您無法在尚未主控 DNS 的現有網域中，安裝具有 DNS 的 Active Directory 網域服務         |                                                                                                                      *無法取得此錯誤*                                                                                                                      |
|     62     |                                         回應檔案中沒有 [DCInstall] 區段                                          |                                                                                             *只有在已被取代的 dcpromo/unattend 中才看得到。請參閱較舊的檔。*                                                                                              |
|     63     |                                       樹系功能等級低於 Windows Server 2003                                       |                                                            請將樹系功能等級至少提高至 Windows Server 2003 原生。 已不再支援 Windows 2000 與 Windows NT 4.0 作業系統                                                             |
|     64     |                                      升級失敗，因為元件二進位偵測失敗                                      |                                                                                                                           請安裝 AD DS 角色                                                                                                                           |
|     65     |                                    升級失敗，因為元件二進位安裝失敗                                     |                                                                                                                           請安裝 AD DS 角色                                                                                                                           |
|     66     |                                      升級失敗，因為作業系統偵測失敗                                      |                                  請檢查延伸錯誤和記錄檔。伺服器無法傳回其作業系統版本。 可能必須重新安裝電腦，因為高度懷疑電腦的整體健康狀況不佳                                   |
|     68     |                                                  複寫協力電腦無效                                                  |                                                                             使用 repadmin 或**get-adreplication\\** \* Windows PowerShell 來驗證夥伴網域控制站的健全狀況                                                                              |
|     69     |                                    必要的連接埠已由其他應用程式使用                                     |                                                                                    請使用 **netstat.exe anob** 尋找錯誤地指派給保留 AD DS 連接埠的處理程序                                                                                     |
|     70     |                                          樹系根網域控制站必須是 GC                                          |                                                                                              *只有在已被取代的 dcpromo/unattend 中才看得到。請參閱舊版檔*                                                                                              |
|     71     |                                                 已安裝 DNS 伺服器                                                  |                                                                                          如果已安裝 DNS 伺服器，請勿指定安裝 DNS ( **-installDNS**)                                                                                           |
|     72     |                                  電腦正以非系統管理模式執行遠端桌面服務                                   |        您無法升級此網域控制站，因為它也是針對兩個以上的系統管理員使用者設定的 RDS 伺服器。 請勿在未仔細清查 RDS 的使用狀況之前將它移除。如果應用程式或使用者正在使用它，移除它將會導致中斷         |
|     73     |                                        指定的樹系功能等級無效。                                         |                                                                                                                  請指定有效的樹系功能等級                                                                                                                   |
|     74     |                                        指定的網域功能等級無效。                                         |                                                                                                                  請指定有效的網域功能等級                                                                                                                   |
|     75     |                                   無法判斷預設的密碼複寫原則。                                   |                                                                                                請驗證 RODC 密碼複寫原則存在並且可以存取                                                                                                 |
|     76     |                                 指定的複寫/非複寫安全性群組無效                                  |                                                                                請在指定密碼複寫原則時驗證您輸入的網域和使用者帳戶有效                                                                                |
|     77     |                                                指定的引數無效                                                 |                                                                                                                    請檢查延伸錯誤和記錄檔                                                                                                                     |
|     78     |                                            無法檢查 Active Directory 樹系                                             |                                                                                                                    請檢查延伸錯誤和記錄檔                                                                                                                     |
|     79     |                                 無法升級 RODC，因為尚未執行 rodcprep                                  |                                                                                               請使用 Windows Server 2012 準備樹系或使用 **adprep.exe /rodcprep**                                                                                                |
|     80     |                                                尚未執行準備網域                                                 |                                                                                              請使用 Windows Server 2012 準備網域或使用 **adprep.exe /domainprep**                                                                                               |
|     81     |                                                尚未執行 Forestprep                                                 |                                                                                              請使用 Windows Server 2012 準備樹系或使用 **adprep.exe /forestprep**                                                                                               |
|     82     |                                                      樹系架構不符                                                      |                                                                                              請使用 Windows Server 2012 準備樹系或使用 **adprep.exe /forestprep**                                                                                               |
|     83     |                                                         不支援的 SKU                                                          |                                                                                                                       *不可能收到此錯誤*                                                                                                                       |
|     84     |                                           無法偵測到網域控制站帳戶                                           |                                                                                         請驗證現有網域控制站具有正確的使用者帳戶控制屬性集。                                                                                         |
|     85     |                                     無法在階段 2 選取網域控制站帳戶                                     |                                                 會在您指定 「 使用現有帳戶 」但沒有找到帳戶或帳戶查閱期間發生錯誤時傳回。 請確定您提供的 RODC 分段帳戶正確                                                 |
|     86     |                                                  需執行階段 2 升級                                                   |                                                                       會在您升級其他網域控制站但有現有帳戶存在且未指定「允許重新安裝」時傳回                                                                       |
|     87     |                                      有類型衝突的網域控制站帳戶存在                                      |   如果不是要嘗試附加到未佔用的網域控制站，請先將電腦重新命名之後再升級。 視帳戶類型而定，您必須使用 **-useexistingaccount**和正確的唯讀或可寫入引數，來附加至未佔用的網域控制站帳戶    |
|     88     |                                             指定的伺服器系統管理員無效                                              |                                                                           您指定的 RODC 管理委派帳戶無效。 請確定指定的帳戶是有效的使用者或群組                                                                           |
|     89     |                                         指定網域的 RID 主機已離線。                                          |                                                                 請使用 **netdom.exe query fsmo** 偵測 RID 主機。 請讓主機上線並設定主機以供您正在升級的網域控制站存取                                                                  |
|     90     |                                                 網域命名主機已離線。                                                 |                                                            請使用 **netdom.exe query fsmo** 偵測網域命名主機。 請讓主機上線並設定主機以供您正在升級的網域控制站存取                                                             |
|     91     |                                             無法偵測處理程序是否為 wow64                                             |                                                                                                  *無法再取得此錯誤，作業系統是64位*                                                                                                  |
|     92     |                                                  不支援 Wow64 處理程序                                                  |                                                                                                  *無法再取得此錯誤，作業系統是64位*                                                                                                  |
|     93     |                                網域控制站服務未執行，無法執行非強制降級                                |                                                                                                                          請啟動 AD DS 服務                                                                                                                           |
|     94     |                           本機系統管理員密碼不符合需求： 空白或非必要                           |                                                                                         請提供非空白密碼並確保本機密碼原則有要求密碼                                                                                         |
|     95     |              無法將即時 RODC 所在之網域中的最後一個 Windows Server 2008 或更新版本的網域控制站降級。              |                                                                             您必須先將所有 RODC 降級，才能將所有 Windows Server 2008 或更新版本的可寫入網域控制站降級                                                                             |
|     96     |                                                 無法解除安裝 DS 二進位檔                                                  |                                                                                              *只有在已被取代的 dcpromo/unattend 中才看得到。請參閱舊版檔*                                                                                              |
|     97     |                      樹系功能等級版本高於子網域作業系統版本                       |                                                                                           提供與樹系功能等級相同或更高的子網域功能                                                                                            |
|     98     |                                        元件二進位安裝/解除安裝正在進行中。                                        |                                                                                              *只有在已被取代的 dcpromo/unattend 中才看得到。請參閱舊版檔*                                                                                              |
|     99     |                              樹系功能等級太低 (只有 Windows Server 2012 才會發生此錯誤)                              |                                                            請將樹系功能等級至少提高至 Windows Server 2003 原生。 已不再支援 Windows 2000 與 Windows NT 4.0 作業系統                                                             |
|    100     |                              網域功能等級太低 (只有 Windows Server 2012 才會發生此錯誤)                              |                                                            請將網域功能等級至少提高至 Windows Server 2003 原生。 已不再支援 Windows 2000 與 Windows NT 4.0 作業系統                                                             |

## <a name="known-issues-and-common-support-scenarios"></a>已知問題和常見的支援案例

以下是 Windows Server 2012 開發程序的過程中常見的問題。 這所有的問題都是「經過設計的」，具有有效的因應措施或更適當的技術，可在第一時間避免發生這些問題。 這些行為中有許多在 Windows Server 2008 R2 和較舊的作業系統中完全相同，但是重寫 AD DS 部署提高了問題的敏感性。  

|問題|將網域控制站降級會讓 DNS 維持在沒有區域的情況下執行|  
|---------|-----------------------------------------------------------------|  
|問題|伺服器仍會回應 DNS 要求但沒有區域資訊|  
|解決方式和注意事項|移除 AD DS 角色時，也會移除 DNS 伺服器角色或將 DNS 伺服器服務設定為停用。 請記得將 DNS 用戶端指向其本身之外的其他伺服器。 如果使用 Windows PowerShell，請在將伺服器降級之後執行以下項目：<br /><br />程式碼-uninstall dns<br /><br />或<br /><br />程式碼集-服務 dns-starttype 已停用<br />停止服務 dns|  

|問題|將 Windows Server 2012 升級至現有單一標籤網域並不會設定 updatetopleveldomain = 1 或 allowsinglelabeldnsdomain = 1|  
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|  
|問題|不會發生 DNS 動態記錄登錄|  
|解決方式和注意事項|使用 Netlogon 與 DNS 群組原則設定這些值。 Microsoft 已開始封鎖在 Windows Server 2008 中建立單一標籤網域。您可以使用 ADMT 或網域重新命名工具來變更為已核准的 DNS 網域結構。|  

|問題|如果有預先建立、未佔用的 RODC 帳戶，降級網域中最後一個網域控制站就會失敗|  
|---------|------------------------------------------------------------------------------------------------------------|  
|問題|降級失敗時會顯示以下訊息：<br /><br />**Dcpromo. General. 54**<br /><br />Active Directory 網域服務找不到另一個 Active Directory 網域控制站以傳輸目錄磁碟分割 CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com 中剩餘的資料。<br /><br />「 指定之網域名稱的格式無效。」|  
|解決方式和注意事項|請先移除任何剩餘之預先建立的 RODC 帳戶，然後再將網域降級 (使用 **Dsa.msc** 或 **Ntdsutil.exe metadata cleanup**)。|  

|問題|自動化樹系和網域準備作業並未執行 GPPREP|  
|---------|---------------------------------------------------------------|  
|問題|群組原則的跨網域計畫功能 (原則結果組 (RSOP) 計劃模式) 需要已針對現有 GP 更新的檔案系統和 Active Directory 權限。 沒有 Gpprep，您就無法跨網域使用 RSOP 計畫。|  
|解決方式和注意事項|請為之前不是為 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 準備的所有網域手動執行 **adprep.exe /gpprep** 。 在網域的紀錄中，系統管理員應只執行一次 GPPrep，而非每次升級都執行。 它不是由自動 Adprep 執行，因為如果您已經設定適當的自訂權限，這會導致在所有網域控制站上複寫所有 SYSVOL 內容。|  

|問題|在指向 UNC 路徑時驗證「從媒體安裝」失敗|  
|---------|------------------------------------------------------------------|  
|問題|傳回錯誤：<br /><br />程式碼-無法驗證媒體路徑。 呼叫 "Oomads.getdatabaseinfo" 和 "2" 引數時發生例外狀況。 資料夾無效。|  
|解決方式和注意事項|您必須將 IFM 檔案儲存在本機磁碟，而非遠端 UNC 路徑。 此刻意區塊可防止因為網路中斷而導致伺服器只有部分升級。|  

|問題|在網域控制站升級期間顯示兩次 DNS 委派警告|  
|---------|-------------------------------------------------------------------------|  
|問題|使用 ADDSDeployment Windows PowerShell 升級時傳回警告*兩次*：<br /><br />程式碼-「無法建立此 DNS 伺服器的委派，因為找不到授權的上層區域，或其未執行 Windows DNS 伺服器。 如果您要與現有的 DNS 基礎結構整合，您應該在父區域中手動建立此 DNS 伺服器的委派，以確保來自網域外部的可靠名稱解析。 否則，就不需要採取任何動作。」|  
|解決方式和注意事項|忽略。 ADDSDeployment Windows PowerShell 會在先決條件檢查時第一次顯示警告，然後在設定網域控制站時再顯示一次。 如果您不想設定 DNS 委派，請使用引數：<br /><br />程式碼--creatednsdelegation： $false<br /><br />請*勿*略過先決條件檢查，以隱藏此訊息|  

|問題|在設定期間指定 UPN 或非網域認證時傳回誤導錯誤|  
|---------|--------------------------------------------------------------------------------------------|  
|問題|伺服器管理員傳回錯誤：<br /><br />程式碼-呼叫 "DNSOption" 與 "6" 引數的例外狀況<br /><br />ADDSDeployment Windows PowerShell 傳回錯誤：<br /><br />使用者權限的程式碼驗證失敗。 您必須提供此使用者帳戶所屬的網功能變數名稱稱。|  
|解決方式和注意事項|請確定您是以**網域 \ 使用者**形式提供有效的網域認證。|  

|問題|使用 Dism.exe 移除 DirectoryServices-DomainController 角色會導致伺服器無法開機|  
|---------|---------------------------------------------------------------------------------------------------|  
|問題|如果在順利降級網域控制站之前使用 Dism.exe 移除 AD DS 角色，伺服器就無法再正常開機並且會顯示錯誤：<br /><br />程式碼-狀態：0x000000000<br />資訊：發生未預期的錯誤。|  
|解決方式和注意事項|使用 *Shift + F8* 開機進入目錄服務修復模式。 重新新增 AD DS 角色，然後強制降級網域控制站。 或者，從備份還原系統狀態。 請勿使用 Dism.exe 來移除 AD DS 角色，此公用程式無法識別網域控制站。|  

|問題|設定 Win2012 的 Forestmode 時安裝新樹系失敗|  
|---------|--------------------------------------------------------------------|  
|問題|使用 ADDSDeployment Windows PowerShell 升級時傳回錯誤：<br /><br />VerifyDcPromoCore. 一般. 74<br /><br />網域控制站升級的必要條件驗證失敗。 指定的網域功能等級無效|  
|解決方式和注意事項|請勿在*未*指定 Win2012 網域功能模式的情況下指定 Win2012 樹系功能模式。 以下是沒有產生錯誤的正常運作範例：<br /><br />程式碼--forestmode Win2012-domainmode Win2012]|  

|||  
|-|-|  
|問題|按一下 [從媒體安裝] 選擇區域中的 [驗證] 之後似乎沒有執行任何作業|  
|問題|當您指定 IFM 資料夾的路徑時，按一下 [驗證] 按鈕時沒有傳回訊息或似乎沒有執行任何動作。|  
|解決方式和注意事項|[驗證] 按鈕只有在有錯誤時才會傳回錯誤。 反之，如果您已提供 IFM 路徑，會讓 [下一步] 按鈕變成可選取。 如果您已選取 IFM，您必須按一下 [驗證] 才能繼續。|  

|||  
|-|-|  
|問題|使用伺服器管理員執行降級作業時，不會在作業完成之前提供任何意見反應。|  
|問題|使用伺服器管理員移除 AD DS 角色及降級網域控制站時，在降級完成或失敗之前不會持續提供任何意見反應。|  
|解決方式和注意事項|這是伺服器管理員的限制。 如需意見反應，請使用 ADDSDeployment Windows PowerShell Cmdlet：<br /><br />程式碼-卸載-uninstall-addsdomaincontroller|  

|||  
|-|-|  
|問題|「從媒體安裝驗證」不會偵測針對可寫入網域控制站提供的 RODC 媒體，反之亦然。|  
|問題|使用 IFM 升級新的網域控制站且提供給 IMF 的媒體不正確時 (例如提供 RODC 媒體給可寫入網域控制站，或提供 RWDC 媒體給 RODC ) [驗證] 按鈕都不會傳回錯誤。 之後，升級會失敗並顯示錯誤：<br /><br />程式碼-嘗試將這部電腦設定為網域控制站時發生錯誤。 <br />因為不允許指定的源資料庫，所以無法啟動唯讀 DC 的從媒體安裝升級。 只有來自其他 Rodc 的資料庫可以用於 RODC 的 IFM 升級。|  
|解決方式和注意事項|驗證時只會驗證 IFM 的整體完整性。 請勿提供錯誤的 IFM 類型給伺服器。 請先重新啟動伺服器，然後使用正確的媒體再次嘗試升級。|  

|||  
|-|-|  
|問題|預先建立之電腦帳戶中的 RODC 升級失敗|  
|問題|使用 ADDSDeployment Windows PowerShell 來升級具備分段電腦帳戶的新 RODC 時，收到錯誤：<br /><br />無法使用指定的具名引數解析程式代碼參數集。    <br />InvalidArgument: ParameterBindingException<br />    + FullyQualifiedErrorId： AmbiguousParameterSet、Microsoft.directoryservices，然後安裝|  
|解決方式和注意事項|請勿提供已在預先建立的 RODC 帳戶中定義的參數。 這些地方包括：<br /><br />程式碼--readonlyreplica<br />-installdns<br />-donotconfigureglobalcatalog<br />-sitename<br />-installdns|  

|||  
|-|-|  
|問題|取消選取/選取 [必要時自動重新啟動目的地伺服器] 時未執行任何動作|  
|問題|如果選取（或不選取）伺服器管理員選項**會在必要時自動重新開機每個目的地伺服器**，以透過移除角色來 whendemoting 網域控制站，不論選擇為何，伺服器一律會重新開機。|  
|解決方式和注意事項|這是刻意設計的。 無論此設定為何，降級處理程序都會重新啟動伺服器。|  

|||  
|-|-|  
|問題|Dcpromo.log 顯示「[錯誤] 伺服器檔案設定安性失敗，發生 2 項失敗」|  
|問題|網域控制站降級完成且未發生問題，但是檢查 dcpromo 記錄時顯示錯誤：<br /><br />程式碼-[錯誤] 在伺服器檔案上設定安全性失敗，發生2|  
|解決方式和注意事項|請忽略，預期會發生此錯誤且沒有實質作用。|  

|||  
|-|-|  
|問題|先決條件 adprep 檢查失敗，發生「 無法執行 Exchange 架構衝突檢查 」錯誤|  
|問題|嘗試將 Windows Server 2012 網域控制站升級到現有的 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 樹系時，先決條件檢查失敗並發生錯誤：<br /><br />AD 準備的必要條件的程式碼驗證失敗。 無法對網域 *<domain name>* 執行 Exchange 架構衝突檢查（例外狀況： RPC 伺服器無法使用）<br /><br />adprep.log 顯示錯誤：<br /><br />Code-Adprep 無法從伺服器取得資料 *<domain controller>*<br /><br />透過 Windows Management Instrumentation （WMI）。|  
|解決方式和注意事項|新的網域控制站無法透過 DCOM/RPC 通訊協定對現有網域控制站存取 WMI。 到目前為止，有三個原因造成此問題：<br /><br />-防火牆規則會封鎖對現有網域控制站的存取<br /><br />-現有網域控制站上的「以服務方式登入」（SeServiceLogonRight）許可權遺漏網路服務帳戶<br /><br />-在網域控制站上停用 NTLM，使用[Ntlm 驗證的限制簡介](https://technet.microsoft.com/library/dd560653(WS.10).aspx)中所述的安全性原則|  

|||  
|-|-|  
|問題|建立新的 AD DS 樹系一律會顯示 DNS 警告|  
|問題|在新的網域控制站上為其本身建立新的 AD DS 樹系及建立 DNS 區域時，您一定會收到警告訊息：<br /><br />程式碼-在 DNS 設定中偵測到錯誤。 <br />此電腦所使用的 DNS 伺服器都不會在逾時間隔內回應。<br />（錯誤碼 0x000005B4 "ERROR_TIMEOUT"）|  
|解決方式和注意事項|忽略。 這個警告是在新樹系之根網域中第一個網域控制站上刻意設計的，以方便您指向現有 DNS 伺服器和區域。|  

|||  
|-|-|  
|問題|Windows PowerShell-whatif 引數傳回不正確的 DNS 伺服器資訊|  
|問題|如果您在設定具備隱含或明確 **-installdns： $true** 的網域控制站時使用 **-whatif** 引數，會顯示以下輸出結果：<br /><br />代碼-「DNS 伺服器：否」|  
|解決方式和注意事項|忽略。 已正確安裝及設定 DNS。|  

|||  
|-|-|  
|問題|升級之後登入失敗，並顯示 [儲存體不足，無法處理此命令。 ]|  
|問題|在您升級新的網域控制站之後，接著登出並嘗試以互動方式登入時，您收到錯誤：<br /><br />沒有足夠的儲存空間可用來處理此命令|  
|解決方式和注意事項|因為發生錯誤或因為您指定了 ADDSDeployment Windows PowerShell 引數 **-norebootoncompletion**，導致網域控制站沒有在升級之後重新開機。 重新啟動網域控制站。|  

|||  
|-|-|  
|問題|[網域控制站選項] 頁面的 [下一步] 按鈕無法使用|  
|問題|即使您已設定密碼，伺服器管理員的 [網域控制站選項] 頁面中還是無法使用 [下一步] 按鈕。 [站台名稱] 功能表中沒有列出任何站台。|  
|解決方式和注意事項|您有多個 AD DS 站台且至少有一個站台遺失子網路。這個新的網域控制站隸屬於那些子網路的其中之一。 您必須從 [站台名稱] 下拉式功能表手動選取子網路。 您也應該使用 DSSITE.MSC 檢閱所有 AD 站台，或使用以下 Windows PowerShell 命令來尋找遺失子網路的所有站台：<br /><br />New-adreplicationsite-filter \*-property 子網&#124; ，其中-object {！ $ _ subnet-eq "\*"} &#124;格式-資料表名稱|  

|||  
|-|-|  
|問題|升級或降級失敗，且顯示 [無法啟動服務] 訊息|  
|問題|如果您嘗試升級、降級，或複製網域控制站，會收到錯誤：<br /><br />程式碼-無法啟動服務，可能是因為它已停用或沒有任何已啟用的裝置」（0x80070422）<br /><br />可能是互動，事件，或寫入記錄檔 (例如 dcpromoui.log 或 dcpromo.log) 錯誤|  
|解決方式和注意事項|DS 角色伺服器服務 (DsRoleSvc) 已停用。 安裝 AD DS 角色時預設會安裝此服務，並設定為手動啟動類型。 請勿停用此服務。 將服務設回手動並允許 DS 角色作業在需要時啟動及停止該服務。 此行為是設計使然。|  

|||  
|-|-|  
|問題|伺服器管理員仍然會顯示您需要升級 DC 的警告|  
|問題|如果您使用過時的 dcpromo.exe /unattend 升級網域控制站，或將現有 Windows Server 2008 R2 網域控制站就地升級至 Windows Server 2012，伺服器管理員仍會顯示部署後組態設定工作：**將此伺服器升級為網域控制站**。|  
|解決方式和注意事項|按一下部署後警告連結，訊息就會消失。 此行為沒有實質作用且是預期的行為。|  

|||  
|-|-|  
|問題|伺服器管理員部署指令碼遺失角色安裝|  
|問題|如果您使用伺服器管理員升級網域控制站並儲存 Windows PowerShell 部署指令碼，它不會包含角色安裝 Cmdlet 與引數 (install-windowsfeature -name ad-domain-services -includemanagementtools)。 無法在沒有角色的情況下設定 DC。|  
|解決方式和注意事項|手動新增該 Cmdlet 與引數至任何指令碼。 此行為是預期的行為且是設計使然。|  

|||  
|-|-|  
|問題|伺服器管理員部署指令碼的名稱不是 PS1|  
|問題|如果您使用伺服器管理員升級網域控制站並儲存 Windows PowerShell 部署指令碼，檔案是以隨機暫時名稱命名，而不是命名為 PS1 檔案。|  
|解決方式和注意事項|手動重新命名檔案。 此行為是預期的行為且是設計使然。|  

|問題|Dcpromo /unattend 允許使用不支援的功能等級|  
|-|-|  
|問題|如果您搭配下列範例回應檔案使用 dcpromo /unattend 來升級網域控制站：<br /><br />錯誤碼<br /><br />DCInstall<br />NewDomain = 樹系<br /><br />ReplicaOrNewDomain = 網域<br /><br />NewDomainDNSName = corp. contoso .com<br /><br />SafeModeAdminPassword =Safepassword@6<br /><br />DomainNetbiosName = corp<br /><br />DNSOnNetwork = 是<br /><br />AutoConfigDNS = 是<br /><br />RebootOnSuccess = NoAndNoPromptEither<br /><br />RebootOnCompletion = 否<br /><br />*DomainLevel = 0*<br /><br />*ForestLevel = 0*<br /><br />升級失敗且 dcpromoui.log 中會顯示下列錯誤：<br /><br />程式碼 dcpromoui.log EA 4.5 B8 0089 13：31： 50.783 Enter CArgumentsSpec：： ValidateArgument DomainLevel<br /><br />dcpromoui.log EA 4.5 B8 008A 13：31： DomainLevel 的50.783 值為0<br /><br />dcpromoui.log EA 4.5 B8 008B 13：31：50.783 結束代碼為77<br /><br />dcpromoui.log EA 4.5 B8 008C 13：31：50.783 指定的引數無效。<br /><br />dcpromoui.log EA 4.5 B8 008D 13：31：50.783 關閉記錄檔<br /><br />dcpromoui.log EA 4.5 B8 0032 13：31：50.830 結束代碼為77<br /><br />等級 0 為 Windows 2000，Windows Server 2012 不支援。|  
|解決方式和注意事項|請勿使用過時的 dcpromo /unattend，而且它允許您指定之後會失敗的無效設定。 此行為是預期的行為且是設計使然。|  

|問題|在建立 NTDS 設定物件時升級「停止回應」，永遠不會完成|  
|-|-|  
|問題|如果您升級複本 DC 或 RODC，升級會到達「建立 NTDS 設定物件」，而且永遠不會繼續或完成。 記錄檔也停止更新。|  
|解決方式和注意事項|這是因提供了與內建網域系統管理員帳戶密碼相同之內建本機系統管理員帳戶的認證而產生的已知問題。 這會導致核心安裝程式引擎失敗，但不會發生錯誤，而是會無限期等待 (odd 迴圈)。 這是預期的-但不需要的行為。<br /><br />修正伺服器：<br /><br />1. 重新開機它。<br /><br />1. 在 AD 中，刪除該伺服器的成員電腦帳戶（它還不會是 DC 帳戶）<br /><br />1. 在該伺服器上，強制從網域退出後它<br /><br />1. 在該伺服器上，移除 AD DS 角色。<br /><br />1. 重新開機<br /><br />1. 重新加入 AD DS 角色並重試升級，確保您一律將***domain\admin***格式的認證提供給 DC 升級，而不只是內建的本機系統管理員帳戶。|  
