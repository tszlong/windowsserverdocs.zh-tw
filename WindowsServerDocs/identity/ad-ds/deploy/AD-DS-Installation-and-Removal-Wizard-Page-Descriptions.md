---
ms.assetid: ac727bd1-a892-47ed-a7ba-439b34187d4e
title: AD DS 安裝和移除精靈頁面說明
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 52e4b215c406eeae11dbab41e367f6ce4cd83507
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849249"
---
# <a name="ad-ds-installation-and-removal-wizard-page-descriptions"></a>AD DS 安裝和移除精靈頁面說明

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題提供下列精靈頁面的控制項說明；這些精靈頁面組成了 [伺服器管理員] 中的 AD DS 伺服器角色安裝和移除。  
  
-   [部署組態](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DepConfigPage)  
  
-   [網域控制站選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)  
  
-   [DNS 選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)  
  
-   [RODC 選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RODCOptionsPage)  
  
-   [其他選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdditionalOptionsPage)  
  
-   [Paths](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Paths)  
  
-   [準備選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdprepCreds)  
  
-   [檢閱選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ViewInstallOptionsPage)  
  
-   [先決條件檢查](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_PrerqCheckPage)  
  
-   [結果](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Results)  
  
-   [角色移除認證](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalCredsPage)  
  
-   [AD DS 移除選項和警告](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalOptionsPage)  
  
-   [新的系統管理員密碼](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_NewAdminPwdPage)  
  
-   [確認角色移除選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ConfirmRoleRemovalPage)  
  
## <a name="BKMK_DepConfigPage"></a>部署組態  
[伺服器管理員] 會從 [部署設定] 頁面開始安裝每個網域控制站。 這個頁面及後續頁面的剩餘選項及必要欄位會隨著您選取的部署操作而變更。 例如，如果您建立新樹系**準備選項**頁面未出現，但是它如果您安裝執行 Windows Server 2012 的現有樹系或網域中的第一個網域控制站。  
  
有些驗證測試是在這個頁面上執行的，而且之後也會在先決條件檢查時進行。 比方說，如果您嘗試在具有 Windows 2000 功能等級的樹系中安裝第一個 Windows Server 2012 網域控制站時，錯誤會出現此頁面上。  
  
建立新樹系時會出現下列選項。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
-   建立新樹系時，必須指定樹系根網域的名稱。 樹系根網域名稱不可以是單一標籤 （例如，它必須是"contoso.com"，而不是"contoso"）。 它必須使用允許的 DNS 網域命名慣例。 您可以指定國際化網域名稱 (IDN)。 如需 DNS 網域命名慣例的詳細資訊，請參閱 [KB 909264](https://support.microsoft.com/kb/909264)。  
  
-   建立新的 Active Directory 樹系時，請不要使用與外部 DNS 名稱相同的名稱。 例如，如果您的網際網路 DNS URL 是 http://contoso.com，您必須選擇不同的名稱，為您的內部樹系以避免未來的相容性問題。 這個名稱必須是唯一而且不像是會用在網路流量的名稱，如 corp.contoso.com。  
  
-   在想要建立新樹系的伺服器上，您必須是該伺服器的 Administrators 群組成員。  
  
如需如何建立樹系的詳細資訊，請參閱[安裝新 Windows Server 2012 Active Directory 樹系&#40;技術等級 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)。  
  
建立新網域時會出現下列選項。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_ChildDomain.gif)  
  
> [!NOTE]  
> 如果您建立新的樹狀目錄網域，需要指定樹系根網域的名稱而不是父系網域的名稱，不過剩餘的精靈頁面和選項是相同的。  
  
-   按一下 [選取] 瀏覽到父系網域或 Active Directory 樹狀結構，或是輸入有效的父系網域或樹狀結構名稱。 接著在 [新網域名稱] 輸入新網域的名稱。  
  
-   樹狀結構網域：提供有效的完整根網域名稱；該名稱不可以是單一標籤，而且必須符合 DNS 網域名稱需求。  
  
-   子網域：提供有效的單一標籤子網域名稱；該名稱必須符合 DNS 網域名稱需求。  
  
-   如果您的目前認證不是來自於網域，[Active Directory 網域服務設定精靈] 會提示您輸入網域認證。 按一下 [變更] 提供網域認證。  
  
如需如何建立網域的詳細資訊，請參閱[安裝新的 Windows Server 2012 Active Directory 子網域或樹狀目錄網域&#40;技術等級 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)。  
  
將新的網域控制站新增到現有網域時，會出現下列選項。  
  
![AD DS 安裝](./media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Replica.gif)  
  
-   按一下 [選取] 瀏覽到網域，或輸入有效的網域名稱。  
  
-   如果需要，[伺服器管理員] 會提示您輸入有效的認證。 安裝其他網域控制站需要 Domain Admins 群組成員資格。  
  
    此外，安裝執行 Windows Server 2012 樹系中的第一個網域控制站需要 Enterprise Admins 與 Schema Admins 群組中加入群組成員資格的認證。 如果您目前的認證不具有適當的權限或群組成員資格，稍後 [Active Directory 網域服務設定精靈] 會提示您。  
  
如需如何將網域控制站新增至現有網域的詳細資訊，請參閱[現有網域中安裝複本 Windows Server 2012 網域控制站&#40;技術等級 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)。  
  
## <a name="BKMK_DCOptionsPage"></a>網域控制站選項  
如果您是建立新的樹系，[網域控制站選項] 頁面會出現這些選項：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Forest.gif)  
  
-   樹系和網域功能等級預設是設定為 Windows Server 2012。  
  
    在 Windows Server 2012 的網域功能等級有可用的一項新功能是： 支援動態存取控制與 Kerberos 防護的 KDC 系統管理範本原則具有兩個設定 （永遠提供宣告與未受防護的驗證失敗要求），需要 Windows Server 2012 網域功能等級。 如需詳細資訊，請參閱 < 支援宣告、 複合驗證以及 Kerberos 防護 > 中[什麼是 Kerberos 驗證的新](https://technet.microsoft.com/library/hh831747.aspx)。    
    Windows Server 2012 樹系功能等級不提供任何新的功能，但是它確保樹系中建立的任何新網域會自動運作在 Windows Server 2012 的網域功能等級。 Windows Server 2012 的網域功能等級不提供任何新的動態存取控制與 Kerberos 防護，支援以外的其他功能，但它可以確保網域中的任何網域控制站執行 Windows Server 2012。 如需不同功能等級可用之其他功能的詳細資訊，請參閱 [了解 Active Directory 網域服務 (AD DS) 功能等級](../active-directory-functional-levels.md)。  
  
    功能等級之外執行 Windows Server 2012 的網域控制站會提供執行舊版 Windows Server 的網域控制站沒有提供的其他功能。 比方說，執行 Windows Server 2012 的網域控制站可以用於虛擬網域控制站複製，而無法執行舊版 Windows Server 的網域控制站。  
  
-   建立新樹系時，預設會選取 DNS 伺服器。 樹系中的第一個網域控制站必須是通用類別目錄 (GC) 伺服器，而不能是唯讀網域控制站 (RODC)。  
  
-   若要登入沒有執行 AD DS 的網域控制站，必須要有目錄服務還原模式 (DSRM) 密碼。 您指定的密碼必須符合適用於伺服器的密碼原則，預設是不需要強式密碼，只需是非空白密碼。 務必選擇複雜的強式密碼，或者最好是使用複雜密碼。 如需如何同步處理 DSRM 密碼與網域使用者帳戶密碼的詳細資訊，請參閱 [KB 961320](https://support.microsoft.com/kb/961320)。  
  
如需如何建立樹系的詳細資訊，請參閱[安裝新 Windows Server 2012 Active Directory 樹系&#40;技術等級 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)。  
  
如果您是建立新的子網域，[網域控制站選項] 頁面上會出現這些選項：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Child.gif)  
  
-   根據預設，網域功能等級是設 Windows Server 2012。 您可以設定任何其他值，這個值必須至少是樹系功能等級的值或更高。  
  
-   可以設定的網域控制站選項包括 [DNS 伺服器] 和 [通用類別目錄]；您不能將唯讀網域控制站設定為新網域中的第一部網域控制站。  
  
    Microsoft 建議所有的網域控制站提供 DNS 和通用類別目錄服務，以便在分散式環境中具有高可用性，這也是為什麼在建立新網域時精靈會預設啟用這些選項的原因。  
  
-   [網域控制站選項] 頁面也能讓您從樹系設定選擇適當的 Active Directory 邏輯 [站台名稱]。 它預設會選取包含最正確子網路的站台。 如果只有一個站台，就會自動選取該站台。  
  
    > [!IMPORTANT]  
    > 如果伺服器不屬於 Active Directory 子網路且有一個以上的站台，則不會選取任何站台，而且會等到您從清單選擇一個站台後，[下一步] 按鈕才可以使用。  
  
如需如何建立網域的詳細資訊，請參閱[安裝新的 Windows Server 2012 Active Directory 子網域或樹狀目錄網域&#40;技術等級 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)。  
  
如果您將網域控制站新增到網域，[網域控制站選項] 頁面上會出現這些選項：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Replica.gif)  
  
-   可設定的網域控制站選項包括 [DNS 伺服器] 和 [通用類別目錄]，以及 [唯讀網域控制站]。  
  
    Microsoft 建議所有的網域控制站提供 DNS 和通用類別目錄服務，以便在分散式環境中具有高可用性，這也是為什麼精靈預設會啟用這些選項的原因。 如需部署 RODC 的詳細資訊，請參閱[唯讀網域控制站規劃和部署指南](https://technet.microsoft.com/library/cc771744(v=WS.10).aspx)。  
  
如需如何將網域控制站新增至現有網域的詳細資訊，請參閱[現有網域中安裝複本 Windows Server 2012 網域控制站&#40;技術等級 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)。  
  
## <a name="BKMK_DNSOptionsPage"></a>DNS 選項  
如果您安裝 DNS 伺服器，會出現下列 [DNS 選項] 頁面：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DNSOptions_Replica.gif)  
  
當您安裝 DNS 伺服器時，應該會在父系網域名稱系統 (DNS) 區域中，為該區域建立指向授權 DNS 伺服器的委派記錄。 委派記錄會轉送名稱解析授權單位，並提供正確轉介給其他 DNS 伺服器和用來做為新區域授權伺服器的新伺服器用戶端。 這些資源記錄包括下列各項：  
  
-   名稱伺服器 (NS) 資源記錄，此記錄可讓委派生效。 這個資源記錄會發出通告，說明名稱為 ns1.na.example.microsoft.com 的伺服器是所委派子網域的授權伺服器。  
  
-   主機 （A 或 AAAA） 資源記錄也稱為黏附記錄必須要有名稱伺服器 (NS) 資源記錄，其 IP 位址中指定的伺服器名稱解析。 將此資源記錄中的主機名稱解析成名稱伺服器 (NS) 資源記錄中所委派的 DNS 伺服器的程序，有時也稱為「黏附追蹤」。  
  
您可以讓 [Active Directory 網域服務設定精靈] 自動建立它們。 在您按一下 [網域控制站選項] 頁面上的 [下一步] 之後，精靈會確認父系 DNS 區域中有無適當的記錄存在。 如果精靈無法確認父系網域中是否有記錄存在，精靈上就會提供選項讓您為新的網域自動建立新的 DNS 委派 (或更新現有委派)，然後繼續進行新的網域控制站安裝。  
  
或者，您可以先建立這些 DNS 委派記錄，然後才安裝 DNS 伺服器。 若要建立區域委派，請開啟 [DNS 管理員]，在父系網域上按一下滑鼠右鍵，然後按一下 [新增委派]。 請遵循新增委派精靈中的步驟來建立委派。  
  
安裝程序會嘗試建立委派，以確認其他網域中的電腦可以解析 DNS 子網域中主機 (包含網域控制站和成員電腦) 的 DNS 查詢。 請注意，委派記錄只能在 Microsoft DNS 伺服器自動建立。 如果父系 DNS 網域區域位於協力廠商 DNS 伺服器 (如 BIND) 上，[先決條件檢查] 頁面就會出現無法建立 DNS 委派記錄的警告。 如需警告的詳細資訊，請參閱[安裝 AD DS 的已知問題](https://technet.microsoft.com/library/cc754463(v=WS.10).aspx)。  
  
父系網域與升級之子網域之間的委派，可以在安裝前或安裝後建立及驗證。 由於您無法建立或更新 DNS 委派，因此沒有必要推遲安裝新網域控制站。  
  
如需有關委派的詳細資訊，請參閱 <<c0> [ 了解區域委派](https://go.microsoft.com/fwlink/?LinkId=164773)(https://go.microsoft.com/fwlink/?LinkId=164773)。 如果在您的情況下無法建立區域委派，可以考慮其他方法，從其他網域將名稱解析提供到您網域內的主機。 例如，其他網域的 DNS 系統管理員可以設定條件性轉送、虛設常式區域或次要區域，以解析網域中的名稱。 如需詳細資訊，請參閱下列主題：  
  
-   [了解區域類型](https://go.microsoft.com/fwlink/?LinkID=157399)(https://go.microsoft.com/fwlink/?LinkID=157399)  
  
-   [了解虛設常式區域](https://go.microsoft.com/fwlink/?LinkId=164776)(https://go.microsoft.com/fwlink/?LinkId=164776)  
  
-   [了解轉寄站](https://go.microsoft.com/fwlink/?LinkId=164778)(https://go.microsoft.com/fwlink/?LinkId=164778)  
  
## <a name="BKMK_RODCOptionsPage"></a>RODC 選項  
安裝唯讀網域控制站 (RODC) 時會出現下列選項。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_RODCOptions.gif)  
  
-   委派的系統管理員帳戶可取得對 RODC 的本機管理權限。 這些使用者可以使用相當於本機電腦的 Administrators 群組的權限進行操作。 他們不是 Domain Admins 群組的成員或網域內建的 Administrators 群組的成員。 這個選項對於委派分公司管理權卻不用釋出網域管理權限很有用。 不需要設定管理委派。 如需詳細資訊，請參閱[系統管理員角色區隔](https://technet.microsoft.com/library/cc753170(v=WS.10).aspx)。  
  
-   「密碼複寫原則」可做為存取控制清單 (ACL)。 該原則會決定是否應允許 RODC 快取密碼。 在 RODC 收到已驗證使用者或電腦登入要求之後，它會參照「密碼複寫原則」來判斷是否應該快取帳戶的密碼。 如此一來，這個相同的帳戶之後就可以更快速地登入。  
  
    「密碼複寫原則」(PRP) 列出了密碼可以被快取的帳戶，以及明確拒絕快取其密碼的帳戶。 被允許快取的使用者和電腦帳戶清單，不表示 RODC 一定會快取這些帳戶的密碼。 例如，系統管理員可以預先指定 RODC 將會快取的任何帳戶。 透過這種方式，即使與中樞站台之間的廣域網路 (WAN) 連線中斷時，RODC 仍然可以驗證這些帳戶。  
  
    沒有被允許 (包括隱含) 或拒絕的任何使用者或電腦都不能快取它們的密碼。 如果這些使用者或電腦沒有可寫入網域控制站的存取權，他們就無法存取 AD DS 提供的資源或功能。 如需 PRP 的詳細資訊，請參閱[密碼複寫原則](https://technet.microsoft.com/library/cc730883(v=ws.10).aspx)。 如需管理 PRP 的詳細資訊，請參閱[管理密碼複寫原則](https://technet.microsoft.com/library/rodc-guidance-for-administering-the-password-replication-policy(v=ws.10).aspx)。  
  
如需有關如何安裝 Rodc 的詳細資訊，請參閱 <<c0> [ 安裝 Windows Server 2012 Active Directory 唯讀網域控制站&#40;RODC&#41; &#40;技術等級 200&#41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)。</c0>  
  
## <a name="BKMK_AdditionalOptionsPage"></a>其他選項  
如果您是建立新的網域，[其他選項] 頁面上會出現下列選項：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Child.gif)  
  
如果您是在現有網域中安裝其他網域控制站，[其他選項] 頁面上會出現下列選項：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Replica.gif)  
  
-   您可以指定網域控制站當作複寫來源，或允許精靈選擇任一網域控制站當作複寫來源。  
  
-   您也可以使用「從媒體安裝」(IFM) 選項，使用備份的媒體來安裝網域控制站。 如果安裝媒體儲存在本機，[從媒體安裝路徑] 選項能讓您瀏覽到檔案位置。 遠端安裝無法使用瀏覽選項。 您可以按一下 [驗證] 確定提供的路徑是有效媒體。 必須使用 Windows Server Backup 或 Ntdsutil.exe 建立 IFM 選項使用的媒體，從另一個現有 Windows Server 2012 的電腦建立媒體的 Windows Server 2012 網域控制站，您無法使用 Windows Server 2008 R2 或舊版作業系統。 如果媒體使用 SYSKEY 加以保護，[伺服器管理員] 在驗證期間會提示輸入映像的密碼。  
  
如需如何建立網域的詳細資訊，請參閱[安裝新的 Windows Server 2012 Active Directory 子網域或樹狀目錄網域&#40;技術等級 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)。 如需如何將網域控制站新增至現有網域的詳細資訊，請參閱[現有網域中安裝複本 Windows Server 2012 網域控制站&#40;技術等級 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)。  
  
## <a name="BKMK_Paths"></a>Paths  
[路徑] 頁面上會出現下列選項。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_Paths.gif)  
  
-   [路徑] 頁面能讓您覆寫 AD DS 資料庫、資料庫交易記錄以及 SYSVOL 共用的預設資料夾位置。 預設位置永遠是 %systemroot%。  
  
指定 AD DS 資料庫 (NTDS.DIT)、記錄檔以及 SYSVOL 的位置。 如果是本機安裝，您可以瀏覽到想要儲存檔案的位置。  
  
## <a name="BKMK_AdprepCreds"></a>準備選項  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PreparationOptions.gif)  
  
如果您目前登入所使用的認證不足以執行 adprep.exe 命令，但必須執行 adprep 才能完成 AD DS 安裝的時候，系統會提示您提供認證以執行 adprep.exe。 Adprep 才能執行，以新增到現有的網域或樹系中執行 Windows Server 2012 的第一個網域控制站。 更明確地說：  
  
-   若要新增至現有的樹系中執行 Windows Server 2012 的第一個網域控制站，必須執行 Adprep /forestprep。 您必須是裝載架構主機之網域的 Enterprise Admins 群組、Schema Admins 群組及 Domain Admins 群組的成員，才可以執行這個命令。 若要成功完成這個命令，執行命令的電腦與樹系的架構主機之間必須連線。  
  
-   若要新增至現有的網域中執行 Windows Server 2012 的第一個網域控制站，就必須執行 Adprep /domainprep。 在您要安裝執行 Windows Server 2012 的網域控制站之網域的 Domain Admins 群組的成員必須執行這個命令。 若要成功完成這個命令，執行命令的電腦與網域的基礎結構主機之間必須連線。  
  
-   必須執行 Adprep /rodcprep 才能將第一部 RODC 新增到現有樹系。 這個命令必須由 Enterprise Admins 群組的成員執行。 若要成功完成這個命令，執行命令的電腦與樹系中每個應用程式目錄分割的基礎結構主機之間必須連線。  
  
如需 Adprep.exe 的詳細資訊，請參閱[Adprep.exe 整合](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep)並查看[執行 Adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx)。  
  
## <a name="BKMK_ViewInstallOptionsPage"></a>檢閱選項  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_ReviewOptions.gif)  
  
-   [檢閱選項]  頁面能讓您驗證設定，並確保它們符合您的需求，然後才開始安裝。 這不是使用 [伺服器管理員] 停止安裝的最後機會。 這個頁面只是讓您檢閱並確認設定，然後才繼續設定。  
  
-   [伺服器管理員] 中的 [檢閱選項]  頁面也提供選用的 [檢視指令碼]  按鈕，用來建立一個包含目前的 ADDSDeployment 設定的 Unicode 文字檔，以便做為一個 Windows PowerShell 指令碼。 這樣可以讓您將 [伺服器管理員] 的圖形介面當作 Windows PowerShell 部署工作室一樣操作。 使用 [Active Directory 網域服務設定精靈] 來設定選項、匯出設定，然後取消精靈。 這個程序會建立一個有效且合乎語義的正確範例，以備日後修改或直接使用。  
  
## <a name="BKMK_PrerqCheckPage"></a>先決條件檢查  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PrerequisitesCheck.gif)  
  
這個頁面上會出現的一些警告如下：  
  
-   執行 Windows Server 2008 或更新版本中有 「 允許的密碼編譯演算法與 Windows NT 4 相容 」 的預設設定，以防止較弱的密碼編譯演算法，建立安全通道工作階段時的網域控制站。 如需可能的影響及解決方法的詳細資訊，請參閱 KB 文章 [942564](https://support.microsoft.com/kb/942564)。  
  
-   無法建立或更新 DNS 委派。 如需詳細資訊，請參閱 [DNS 選項](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)。  
  
-   先決條件檢查需要 WMI 呼叫。 如果它們被防火牆規則封鎖就會失敗，並傳回 RPC 伺服器無法使用錯誤。  
  
如需針對 AD DS 安裝所執行的特定先決條件檢查詳細資訊，請參閱[先決條件測試](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_ADDSInstallPrerequisiteTests)。  
  
## <a name="BKMK_Results"></a>結果  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_SMResultsBeta.gif)  
  
您可以在這個頁面上檢閱安裝的結果。  
  
您也可以選擇在精靈完成後重新啟動目標伺服器，但是如果安裝成功的話，無論您是否選取該選項，伺服器一律都會重新啟動。 在某些情況下，如果在安裝前就在未加入網域的目標伺服器上完成精靈，目標伺服器的系統狀態會讓它無法連線網路，或系統狀態可能阻止您取得管理遠端伺服器的權限。  
  
如果目標伺服器無法在這種情況下重新啟動，您必須手動重新啟動它。 shutdown.exe 或 Windows PowerShell 之類的工具無法重新啟動它。 您可以使用遠端桌面服務登入目標伺服器並從遠端將它關機。  
  
## <a name="BKMK_RemovalCredsPage"></a>角色移除認證  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Credentials.gif)  
  
您可以在 [認證]  頁面上設定降級選項。 提供執行下列清單降級所需的認證：  
  
-   降級其他網域控制站需要 Domain Admin 認證。 選取**強制移除網域控制站**會降級網域控制站，但不從 Active Directory 移除網域控制站物件的中繼資料。  
  
    > [!IMPORTANT]  
    > 請不要選取這個選項，除非網域控制站無法連絡其他網域控制站，而且沒有其他正當的方法可以解決這個網路問題。 強制降級會在樹系的剩餘網域控制站上的 Active Directory 中留下孤立的中繼資料。 不僅如此，該網域控制站上所有未複寫的變更 (如密碼或新的使用者帳戶) 都會永遠遺失。 孤立的中繼資料是 Microsoft 客戶支援遇到大部分 AD DS、Exchange、SQL 及其他軟體問題的根本原因。 如果您強制降級網域控制站，則必須立即手動清理中繼資料。 如需相關步驟，請參閱 [清理伺服器中繼資料](https://technet.microsoft.com/library/cc816907(WS.10).aspx)。  
  
-   降級網域中最後一部網域控制站需要 Enterprise Admins 群組成員資格，因為它會移除網域本身 (如果是樹系的最後一個網域，則會移除樹系)。 如果您目前的網域控制站是網域的最後一部網域控制站，[伺服器管理員] 將會通知您。 選取 [網域中的最後一個網域控制站]，確認網域控制站是網域中的最後一個網域控制站。  
  
如需有關如何移除 AD DS 的詳細資訊，請參閱 <<c0> [ 移除 Active Directory 網域服務 (技術等級 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)並[降級網域控制站和網域&#40;技術等級 200&#41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md)。</c0>  
  
## <a name="BKMK_RemovalOptionsPage"></a>AD DS 移除選項和警告  
如果您在 [檢閱選項] 頁面上需要協助，請參閱＜檢閱選項＞  
  
如果網域控制站裝載其他角色，如 DNS 伺服器角色或通用類別目錄伺服器，會出現下方的 [警告] 頁面：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Warnings.gif)  
  
您必須按一下 [繼續移除] 了解其他角色已無法使用，才能按 [下一步] 繼續。  
  
如果您強制移除網域控制站，將會遺失任何沒有複寫到網域中其他網域控制路的 Active Directory 物件變更。 此外，如果網域控制站裝載了操作主機角色、通用類別目錄或 DNS 伺服器角色，則可能也會影響網域和樹系中的重大操作，如下所示。 移除裝載任何操作主機角色的網域控制站之前，請試著將角色轉移到另一個網域控制站。 如果無法轉移角色，請先從這部電腦移除 Active Directory 網域服務，然後使用 Ntdsutil.exe 拿取角色。 在打算拿取角色的網域控制站上使用 Ntdsutil；如果可以的話，使用與這個網域控制站位於相同站台中的最新複寫夥伴。 如需轉移和拿取操作主機角色的詳細資訊，請參閱 Microsoft 知識庫[文章 255504](https://go.microsoft.com/fwlink/?LinkId=80395)。 如果精靈無法判斷網域控制站是否裝載了操作主機角色，請執行 netdom.exe 命令來判斷這個網域控制站是否執行任何操作主機角色。  
  
-   通用類別目錄：使用者可能會無法登入樹系中的網域。 移除通用類別目錄伺服器之前，請確定這個樹系和站台中有足夠的通用類別目錄伺服器可以服務使用者登入。 如有必要，指定另一個通用類別目錄伺服器，並使用這個新的資訊更新用戶端和應用程式。  
  
-   DNS 伺服器：所有儲存在 Active Directory 整合區域的 DNS 資料將會遺失。 移除 AD DS 之後，這個 DNS 伺服器將無法為整合 Active Directory 的 DNS 區域執行名稱解析。 因此，建議您更新目前參照此 DNS 伺服器之 IP 位址的所有電腦的 DNS 設定，以便對新 DNS 伺服器的 IP 位址執行名稱解析。  
  
-   基礎結構主機：網域中的用戶端可能無法找到其他網域中的物件。 在您繼續之前，請將基礎結構主機轉移到不是通用類別目錄伺服器的網域控制站。  
  
-   RID 主機：您可能無法建立新的使用者帳戶、電腦帳戶以及安全性群組。 在您繼續之前，請先將 RID 主機角色轉移到與這個網域控制站位於相同網域中的網域控制站。  
  
-   主要網域控制站 (PDC) 模擬器：由 PDC 模擬器執行的操作，如群組原則更新和重設非 AD DS 帳戶的密碼，將無法正常運作。 在您繼續之前，請先將 PDC 模擬器主機角色轉移到與這個網域控制站位於相同網域中的網域控制站。  
  
-   架構主機：您將無法再修改這個樹系的架構。 在您繼續之前，請先將架構主機角色轉移到樹系根網域中的網域控制站。  
  
-   網域命名主機：您將無法再從這個樹系新增或移除網域。 在您繼續之前，請先將網域命名主機角色轉移到樹系根網域中的網域控制站。  
  
-   這個 Active Directory 網域控制站上的所有應用程式目錄分割將會被移除。 如果網域控制站保留一或多個應用程式目錄分割的最後一個複本，那麼當移除操作完成時，這些分割將不再存在。  
  
請注意，從網域中的最後一個網域控制站上解除安裝 Active Directory 網域服務之後，網域將不再存在。  
  
如果網域控制站是委派來裝載 DNS 區域的 DNS 伺服器，則下方的頁面將提供從 DNS 區域委派移除 DNS 伺服器的選項。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_RemovalOptions.gif)  
  
如需有關如何移除 AD DS 的詳細資訊，請參閱 <<c0> [ 移除 Active Directory 網域服務 (技術等級 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)並[降級網域控制站和網域&#40;技術等級 200&#41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md)。</c0>  
  
## <a name="BKMK_NewAdminPwdPage"></a>新的系統管理員密碼  
**新的系統管理員密碼**頁面會要求您提供的內建的本機電腦的系統管理員帳戶的密碼，一旦降級完成且電腦成為網域成員伺服器或工作群組電腦。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_NewAdminPwd.gif)  
  
如需有關如何移除 AD DS 的詳細資訊，請參閱 <<c0> [ 移除 Active Directory 網域服務 (技術等級 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)並[降級網域控制站和網域&#40;技術等級 200&#41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md)。</c0>  
  
## <a name="BKMK_ConfirmRoleRemovalPage"></a>檢閱選項  
[檢閱選項] 頁面可以讓您將降級組態設定匯出到 Windows PowerShell 指令碼，以便自動化其他的降級。 按一下 [降級] 移除 AD DS。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_ReviewOptions.gif)  
  


