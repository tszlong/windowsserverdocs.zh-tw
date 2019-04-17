---
ms.assetid: ac727bd1-a892-47ed-a7ba-439b34187d4e
title: "AD DS 安裝並移除精靈頁面描述"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fa023398822e79ca8c3e93d44bb1e87fc9190cee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-installation-and-removal-wizard-page-descriptions"></a>AD DS 安裝並移除精靈頁面描述

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供描述 AD DS 伺服器角色安裝和移除在伺服器管理員中包含下列精靈頁面上的控制項。  
  
-   [部署設定](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DepConfigPage)  
  
-   [網域控制站選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)  
  
-   [DNS 選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)  
  
-   [RODC 選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RODCOptionsPage)  
  
-   [其他選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdditionalOptionsPage)  
  
-   [路徑](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Paths)  
  
-   [準備選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdprepCreds)  
  
-   [檢視選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ViewInstallOptionsPage)  
  
-   [必要條件核取](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_PrerqCheckPage)  
  
-   [結果](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Results)  
  
-   [角色移除認證](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalCredsPage)  
  
-   [AD DS 移除選項與警告](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalOptionsPage)  
  
-   [新的系統管理員密碼](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_NewAdminPwdPage)  
  
-   [確認角色移除選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ConfirmRoleRemovalPage)  
  
## <a name="BKMK_DepConfigPage"></a>部署設定  
伺服器管理員會開始使用每個網域控制站安裝**部署組態**頁面。 剩餘的選項與所需的欄位變更此頁面上，後續的部署操作根據您選擇的頁面。 例如，如果您建立新的樹系，**準備選項**頁面上未顯示，但如果您在執行 Windows Server 2012 現有的樹系或網域中的第一個網域控制站安裝，如此。  
  
在這個頁面上，以及稍後再試一次的一部分必要條件檢查執行一些驗證測試。 例如，如果您嘗試重新安裝 Windows 2000 的功能層級的森林中的第一個 Windows Server 2012 網域控制站，此頁面上會出現錯誤。  
  
當您建立新的樹系時，會顯示下列選項。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
-   當您建立新的樹系時，您必須指定樹系根網域的名稱。 單一標示無法森林根網域名稱（例如，它必須」contoso.com「取代"contoso"）。 允許的 DNS 網域命名規格必須使用。 您可以指定國際化網域名稱 (IDN)。 如需 DNS 網域命名規格的詳細資訊，請查看[KB 909264](https://support.microsoft.com/kb/909264)。  
  
-   無法建立新的 Active Directory 森林與您的外接式 DNS 名稱相同的名稱。 例如 http://contoso.com DNS URL 網際網路時，您必須選擇不同的名稱為內部樹系避免未來的相容性問題。 唯一和的網路流量，例如 corp.contoso.com，應該該名稱。  
  
-   您必須是您想要用來建立新的樹系的伺服器上的系統管理員群組成員。  
  
如需如何建立的樹系的詳細資訊，請查看[安裝新 Windows Server 2012 Active Directory 樹系和 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
當您建立新的網域時，會顯示下列選項。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_ChildDomain.gif)  
  
> [!NOTE]  
> 如果您建立新的網域樹，您需要家長網域中，而森林根網域名稱指定但其餘的精靈及選項相同。  
  
-   按一下**選擇**來瀏覽至父系網域或 Active Directory 樹，或輸入有效的父網域或樹名稱。 然後輸入名稱的新的網域中的**新的網域名稱**。  
  
-   樹網域：提供有效的、完整根網域名稱。必須使用 DNS 網域名稱需求和不是單一標記名稱。  
  
-   子女網域：提供有效的、單一標籤子女網域名稱。名稱必須使用 DNS 網域名稱需求。  
  
-   Active Directory Domain Services 組態精靈會提示您輸入網域認證如果您目前的憑證並非來自網域。 按一下**變更**提供網域認證。  
  
如需如何建立網域的詳細資訊，請查看[安裝新 Windows Server 2012 Active Directory 子女或樹網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
當您現有的網域新增新的網域控制站出現下列選項。  
  
![AD DS 安裝](./media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Replica.gif)  
  
-   按一下**選擇**來瀏覽至網域，或輸入正確的網域名稱。  
  
-   伺服器管理員會提示您輸入正確的認證視。 安裝其他網域控制站需要網域系統管理員群組成員資格。  
  
    此外，安裝 Windows Server 2012 上執行的樹系的第一個網域控制站需要包含群組成員資格群組企業系統管理員和架構系統管理員認證。 Active Directory Domain Services 組態精靈會提示您稍後如果您目前的認證，不需要的適當權限或群組成員資格。  
  
如需有關如何將現有的網域網域控制站的詳細資訊，請查看[安裝複本 Windows Server 2012 網域控制站在現有的網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DCOptionsPage"></a>網域控制站選項  
如果您要建立新的樹系，網域控制站選項] 頁面就會有這些選項：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Forest.gif)  
  
-   樹系和網域功能層級設定為 Windows Server 2012 預設。  
  
    還有一個新功能可網域層級 Windows Server 2012 功能：支援動態存取控制和 Kerberos 保護 \ [KDC 系統管理範本原則有兩種設定（永遠提供宣告和失敗護身的驗證要求）需要 Windows Server 2012 網域功能層級。 如需詳細資訊，看到「支援宣告、複合驗證以及 Kerberos 保護 \] [F:kerberos 驗證中的新功能](https://technet.microsoft.com/library/hh831747.aspx)。    
    Windows Server 2012 森林功能層級不提供任何新的功能，但確保任何新的網域建立森林中將會自動操作網域層級 Windows Server 2012 正常運作。 Windows Server 2012 網域功能等級不提供任何新旁邊動態存取控制和 Kerberos 保護 \，支援其他功能，但確保網域中的任何網域控制站執行 Windows Server 2012。 如需有關其他功能，可正常運作的不同層級，請查看[Active Directory Domain Services 了解 (AD DS) 功能的層級](../active-directory-functional-levels.md)。  
  
    功能層級以外執行 Windows Server 2012」的網域控制站提供並不適用於執行較舊版本的 Windows Server 的網域控制站的額外功能。 例如，執行 Windows Server 2012」的網域控制站可用於 virtual 網域控制站複製，而無法執行較舊版本的 Windows Server 的網域控制站。  
  
-   當您建立新的樹系預設會選取 DNS 伺服器。 森林中的第一個網域控制站必須通用 (GC) 伺服器，並無法讀取只有網域控制站 (RODC)。  
  
-   為了 AD DS 不執行為網域控制站登入需要 Directory 服務還原模式 (DSRM) 密碼。 指定的密碼必須遵守密碼原則套用到伺服器，預設不需要穩固密碼。僅限非空白密碼。 隨時複雜的密碼或最好複雜密碼。 有關如何使用密碼的使用者核對同步 DSRM 密碼，請查看[KB 961320](https://support.microsoft.com/kb/961320)。  
  
如需如何建立的樹系的詳細資訊，請查看[安裝新 Windows Server 2012 Active Directory 樹系和 #40;層級 200 和 #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
如果您的子女網域，網域控制站選項] 頁面就會有這些選項：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Child.gif)  
  
-   網域功能等級到 Windows Server 2012 預設設定。 您可以指定至少值的樹系功能等級或更高的任何其他值。  
  
-   包含可設定的網域控制站選項**的 DNS 伺服器**和**通用**。您無法在新的網域中的第一個網域控制站設定唯讀網域控制站。  
  
    Microsoft 建議的所有網域控制站都提供 DNS 和通用服務的可用性分散式的環境中，這是精靈可讓這些選項預設建立新的網域時的原因。  
  
-   **網域控制站選項**頁面上也可讓您選擇適當的 Active Directory 邏輯**網站名稱**的樹系設定。 根據預設，它會選取最正確的子網路的網站。 只有一個網站時，它會選取該網站自動。  
  
    > [!IMPORTANT]  
    > 如果伺服器不屬於 Active Directory 子網路，而且有一個以上的網站，就選取任何項目和**下一步**按鈕，才可使用網站從清單中選擇。  
  
如需如何建立網域的詳細資訊，請查看[安裝新 Windows Server 2012 Active Directory 子女或樹網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
如果您的網域控制站加入網域，網域控制站選項] 頁面就會有這些選項：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Replica.gif)  
  
-   包含可設定的網域控制站選項**的 DNS 伺服器**和**通用**，並**唯讀網域控制站**。  
  
    Microsoft 建議的所有網域控制站都提供 DNS 和通用服務的可用性分散式的環境中，這是精靈預設讓這些選項的原因。 如需部署 Rodc 的相關資訊，請查看[唯讀網域控制站規劃和部署指南](https://technet.microsoft.com/library/cc771744(v=WS.10).aspx)。  
  
如需有關如何將現有的網域網域控制站的詳細資訊，請查看[安裝複本 Windows Server 2012 網域控制站在現有的網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DNSOptionsPage"></a>DNS 選項  
如果您安裝的 DNS 伺服器，下列**DNS 選項**頁面上會出現：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DNSOptions_Replica.gif)  
  
當您安裝的 DNS 伺服器時的 DNS 伺服器區授權按一下委派記錄中建立家長網域名稱系統」(DNS) 區域。 委派記錄傳輸名稱解析授權，並提供給其他 DNS 伺服器和固定的新的伺服器來做為新的區域授權正確推薦。 這些資源記錄包含下列類型：  
  
-   可讓委派生效名稱（奈秒）伺服器資源記錄。 此資源記錄通知 ns1.na.example.microsoft.com 指定的伺服器會委派子授權伺服器。  
  
-   主機（或 AAAA）資源記錄也就是黏附記錄必須要有解析為其 IP 位址名稱（奈秒）伺服器資源記錄中指定的伺服器的名稱。 在這個資源記錄主機名稱解析委派 DNS 伺服器的名稱（奈秒）伺服器資源記錄中的程序是有時稱為「黏住搜尋」。  
  
您可以讓 Active Directory Domain 服務設定精靈會自動建立它們。 精靈會驗證適當的記錄存在家長 DNS 區域之後，您可以按一下**下一步**在**網域控制站選項**頁面。 如果精靈無法確認記錄存在家長網域中，為您提供的選項來建立新的網域新增 DNS 委派（或更新現有委派）自動精靈，並繼續使用新的網域控制站安裝。  
  
或者，您可以建立這些 DNS 委派記錄之前您安裝的 DNS 伺服器。 若要建立區域委派，請打開**DNS 管理員**，家長網域中，按一下滑鼠右鍵，然後按一下**新增委派**。 請依照下列步驟建立委派新委派精靈中。  
  
建立確定主機，包括網域控制站和成員電腦子 DNS 網域中的其他網域中的電腦可以解析 DNS 查詢委派嘗試安裝程序。 請注意委派記錄可以自動建立僅在 Microsoft DNS 伺服器。 如果家長 DNS 網域區域位於像是結第三方 DNS 伺服器，建立 DNS 委派記錄失敗的相關警告顯示必要條件核取頁面上。 如需警告，請查看[的已知問題，適用於安裝 AD DS](https://technet.microsoft.com/library/cc754463(v=WS.10).aspx)。  
  
建立和驗證前後安裝父系網域之間升級子委派。 還有延遲新的網域控制站安裝，因為您無法建立或更新 DNS 委派理由。  
  
如需委派的詳細資訊，請查看[了解區域委派](https://go.microsoft.com/fwlink/?LinkId=164773)(https://go.microsoft.com/fwlink/?LinkId=164773)。 區域委派不能在您的情形，如果您可能會考慮提供您的網域中的主機從其他網域名稱解析其他方法。 例如的另一個網域 DNS 系統管理員可設定條件轉寄、stub 區域或第二個才能在您的網域名稱解析區域。 如需詳細資訊，下列主題：  
  
-   [了解區域類型](https://go.microsoft.com/fwlink/?LinkID=157399)(https://go.microsoft.com/fwlink/?LinkID=157399)  
  
-   [了解 stub 區域](https://go.microsoft.com/fwlink/?LinkId=164776)(https://go.microsoft.com/fwlink/?LinkId=164776)  
  
-   [了解轉送程式](https://go.microsoft.com/fwlink/?LinkId=164778)(https://go.microsoft.com/fwlink/?LinkId=164778)  
  
## <a name="BKMK_RODCOptionsPage"></a>RODC 選項  
當您安裝唯讀網域控制站 (RODC) 時，會顯示下列選項。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_RODCOptions.gif)  
  
-   帳號委派的系統管理員取得 RODC 本機系統管理員權限。 這些使用者可以運作權限相當於在本機電腦的系統管理員」群組。 他們並不網域系統管理員」的網域建系統管理員群組成員。 這個選項適用於分支 office 的管理委派不提供網域系統管理員權限。 設定的管理委派就不需要的。 如需詳細資訊，請查看[系統管理員角色分離](https://technet.microsoft.com/library/cc753170(v=WS.10).aspx)。  
  
-   密碼複寫原則做為存取控制 (ACL) 清單中。 它會判斷是否應該 RODC 允許快取的密碼。 RODC 收到的驗證的使用者或電腦的登入要求之後，它是指密碼複寫原則，以判斷是否應該快取 account 的密碼。 相同 account 然後可以更有效率地執行後續登入。  
  
    密碼複寫原則 (PRP) 列出帳號，允許快取，其密碼及密碼明確的快取的拒絕的帳號。 使用者和電腦帳號，允許快取的清單，並不代表 RODC 一定有快取那些帳號的密碼。 系統管理員的身分，例如，指定事先任何帳號，RODC 會快取。 如此一來，RODC 可以進行驗證那些帳號，即使 WAN 連結中樞網站離線。  
  
    所有的使用者或電腦都不允許（包括隱含）或拒絕的執行快取他們的密碼。 如果那些使用者或電腦不能存取寫入網域控制站到，他們無法存取 AD DS 提供資源或功能。 如需 PRP 的詳細資訊，請查看[密碼複寫原則](https://technet.microsoft.com/library/cc730883(v=ws.10).aspx)。 如需有關管理 PRP 的詳細資訊，請查看[管理密碼原則複製](https://technet.microsoft.com/library/rodc-guidance-for-administering-the-password-replication-policy(v=ws.10).aspx)。  
  
如需有關安裝 Rodc 的詳細資訊，請[安裝 Windows Server 2012 Active Directory Read-Only 網域控制站和 #40;RODC 和 #41;與 #40;層級 200 和 #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md).  
  
## <a name="BKMK_AdditionalOptionsPage"></a>其他選項  
下列選項會出現在**的其他選項**頁面上，如果您要建立新的網域：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Child.gif)  
  
下列選項會出現在**的其他選項**頁面上，如果您安裝其他網域控制站現有網域中：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Replica.gif)  
  
-   您可以指定網域控制站為複寫來源，或讓選擇做為來源複寫任何網域控制站精靈。  
  
-   您也可以選擇備份使用安裝媒體 (IFM) 選項從媒體安裝網域控制站使用。 如果在本機儲存的安裝媒體**安裝媒體路徑的**選項可讓您瀏覽到檔案的位置。 瀏覽] 選項不適用於遠端安裝。 您可以按一下**確認**以確保所提供有效的媒體。 必須與 Windows Server 備份或 Ntdsutil.exe 建立媒體由 IFM 選項，從另一部現有 Windows Server 2012 電腦您無法建立 Windows Server 2012 網域控制站媒體使用 Windows Server 2008 R2 或先前的作業系統。 如果 SYSKEY 受保護的媒體，伺服器管理員會在驗證期間影像的密碼提示。  
  
如需如何建立網域的詳細資訊，請查看[安裝新 Windows Server 2012 Active Directory 子女或樹網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md). 如需有關如何將現有的網域網域控制站的詳細資訊，請查看[安裝複本 Windows Server 2012 網域控制站在現有的網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_Paths"></a>路徑  
下列選項會出現在**路徑**頁面。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_Paths.gif)  
  
-   **路徑**頁面上，可讓您覆寫預設資料夾位置的 AD DS 資料庫中資料庫交易登，並 SYSVOL 分享。 預設位置都隱藏資料夾中。  
  
指定的位置 AD DS 資料庫 (NTDS.DIT)、檔案和 SYSVOL 登入。 本機安裝，您可以瀏覽至您想要用來儲存檔案的位置。  
  
## <a name="BKMK_AdprepCreds"></a>準備選項  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PreparationOptions.gif)  
  
如果您不目前登入認證執行 adprep.exe 命令不足的 adprep，才能執行才能完成 AD DS 安裝，以提供認證來執行 adprep.exe 會提示您。 Adprep，才能執行以新增到現有的網域或森林執行 Windows Server 2012 的第一個網域控制站。 更多尤其是：  
  
-   Adprep /forestprep 必須執行以新增第一次執行 Windows Server 2012 現有的樹系的網域控制站。 必須執行這個命令的企業系統管理員群組、架構管理群組和網域系統管理員群組網域裝載架構主機的成員。 成功完成，此命令必須連接您執行的命令的電腦之間的樹系主機。  
  
-   Adprep /domainprep 必須執行以新增第一次執行 Windows Server 2012 現有網域控制站。 必須執行這個命令的網域安裝執行 Windows Server 2012」的網域控制站的網域管理群組成員。 成功完成，此命令必須連接您執行的命令的電腦之間的基礎結構網域主機。  
  
-   Adprep /rodcprep 加入現有的樹系的第一個 RODC 必須執行。 必須執行這個命令的企業系統管理員群組成員。 成功完成，此命令必須連接您執行的命令的電腦之間的基礎結構主機森林中的每個應用程式 directory 磁碟分割。  
  
如需 Adprep.exe 的詳細資訊，請查看[Adprep.exe 整合](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep)，並查看[執行 Adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx)。  
  
## <a name="BKMK_ViewInstallOptionsPage"></a>檢視選項  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_ReviewOptions.gif)  
  
-   **評論選項**頁面上可讓您驗證您的設定，並確保您開始安裝之前，先其符合您的需求。 這不是一個機會停止使用伺服器管理員安裝。 此頁面上可讓您檢查並確認您的設定，才能繼續設定。  
  
-   **評論選項**在伺服器管理員頁面也提供選擇性**檢視指令碼**按鈕，以建立包含目前 ADDSDeployment 設定成單一的 Windows PowerShell 指令碼 Unicode 文字檔案。 這可讓您在伺服器管理員圖形介面作為 Windows PowerShell 部署 studio。 若要設定選項，匯出設定，然後取消精靈使用 Active Directory Domain Services 組態精靈。 此程序會建立進一步修改或直接使用有效且語法正確範例。  
  
## <a name="BKMK_PrerqCheckPage"></a>必要條件核取  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PrerequisitesCheck.gif)  
  
此頁面會顯示警告包括：  
  
-   執行 Windows Server 2008 或以上版本的 [允許密碼編譯演算法反白 4 相容」的預設設定，以避免較弱密碼編譯演算法，建立安全通道工作階段時的網域控制站。 如需有關潛在的影響，因應措施，查看知識庫文章[942564](https://support.microsoft.com/kb/942564)。  
  
-   DNS 委派無法建立或更新。 如需詳細資訊，請查看[DNS 選項](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)。  
  
-   必要條件檢查需要 WMI 通話。 如果他們會封鎖的防火牆規則封鎖，並傳回 RPC 伺服器它們可能會失敗無法使用的錯誤。  
  
適用於特定的必要條件檢查 AD DS 安裝所執行的相關詳細資訊，請查看[必要條件測試](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_ADDSInstallPrerequisiteTests)。  
  
## <a name="BKMK_Results"></a>結果  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_SMResultsBeta.gif)  
  
在這個頁面上，您可以檢視安裝的結果。  
  
您也可以選取重新開機目標伺服器後完成精靈，但如果成功安裝，伺服器便會一律重新開機無論您是否選擇該選項。 有時候未加入網域，在安裝之前目標伺服器上的精靈完成之後，系統狀態目標伺服器的可以讓伺服器無法存取網路上或系統狀態可讓您有權限管理的遠端伺服器。  
  
如果目標伺服器無法在這種情形下重新開機，您必須手動重新。 工具，例如 shutdown.exe 或 Windows PowerShell 無法將它重新開機。 您可以使用遠端桌面服務來登入，並從遠端關機目標伺服器。  
  
## <a name="BKMK_RemovalCredsPage"></a>角色移除認證  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Credentials.gif)  
  
您在設定降級選項**認證**頁面。 提供從下列清單執行降級所需的認證：  
  
-   降級額外的網域控制站需要網域管理員認證。 選取 [**強制網域控制站的**將網域控制站降級不 Active Directory 中移除網域控制站物件中繼資料。  
  
    > [!IMPORTANT]  
    > 請勿選取此選項，除非網域控制站無法連絡其他網域控制站和有*未合理的方式*解析該網路的問題。 強制的降級離開單獨中繼資料在 Active Directory 中，在森林中的其餘網域控制站。 此外，所有複製未變更密碼] 或 [新增使用者帳號，例如該網域控制站，將會遺失永遠。 單獨中繼資料是在 Microsoft 客戶支援案例的重大百分比 AD DS，Exchange、SQL，及其他軟體的根本原因。 如果您強制降級網域控制站您*必須*以手動方式立即執行中繼資料清除。 步驟，檢視[全新向上伺服器中繼資料](https://technet.microsoft.com/library/cc816907(WS.10).aspx)。  
  
-   降級網域中的最後一個網域控制站需要企業系統管理員群組成員資格，因為這會移除網域（如果這是在森林中的最後一個網域，這會移除樹系）。 如果目前的網域控制站網域中的最後一個網域控制站伺服器管理員會通知您。 選取 [**網域中的最後一個網域控制站**確認網域控制站是網域中的最後一個網域控制站。  
  
如需有關移除 AD DS，請查看[移除 Active Directory Domain Services (層級 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)和[降級網域控制站和網域和 #40;層級 200 和 #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_RemovalOptionsPage"></a>AD DS 移除選項與警告  
如果您需要協助的 [檢視選項] 頁面，查看評論選項  
  
如果您的網域控制站裝載額外的角色，例如 DNS 伺服器角色或通用伺服器，下列警告頁面就會出現：  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Warnings.gif)  
  
您必須按**移除繼續**以確認您的其他角色將不會提供之前，您可以按一下 [**下**繼續。  
  
如果您強制網域控制站移除，已經不會複寫網域中的其他網域控制站任何 Active Directory 物件變更將會遺失。 此外，如果網域控制站裝載作業主機角色、通用或 DNS 伺服器角色，網域森林中的重要操作可能會受到影響，如下所示。 移除網域控制站裝載任何作業主角之前，請試著轉移到另一個網域控制站。 如果您不能傳送的角色，先將 Active Directory Domain Services 移除此電腦]，然後使用 Ntdsutil.exe 抓取角色。 使用 Ntdsutil 網域控制站想要抓取角色。如果可能，請使用這個網域控制站做的最近複寫合作夥伴相同的網站。 為抓取操作主機角色及傳送的相關詳細資訊，請查看[文章 255504](https://go.microsoft.com/fwlink/?LinkId=80395) Microsoft 知識庫中。 如果網域控制站裝載作業主角無法判斷精靈中，執行 netdom.exe 命令，判斷是否這個網域控制站會執行任何操作主機角色。  
  
-   通用：使用者可能無法登入森林中的網域。 移除通用伺服器之前，確定不足，無法通用伺服器此樹系和網站，以服務的使用者登入。 必要時，將另一個通用伺服器指定和更新戶端和應用程式的新資訊。  
  
-   DNS 伺服器：DNS 資料儲存在 Active Directory 整合區域中的所有會遺失。 移除 AD DS 之後，此 DNS 伺服器將無法執行名稱解析已 Active Directory 整合 DNS 區域。 因此，我們建議您更新的所有電腦目前正在參照的名稱解析此 DNS 伺服器的 IP 位址的新的 DNS 伺服器的 IP 位址 DNS 設定。  
  
-   基礎結構主機：戶端網域中的可能不容易找到其他網域中的物件。 您繼續之前，請傳送基礎結構主角網域控制站的不是一個通用伺服器。  
  
-   RID 的主要：您可能有問題建立新的使用者帳號，電腦帳號，並安全性群組。 請您繼續之前，請傳送 RID 主角網域控制站在這個網域控制站相同的網域。  
  
-   主要網域控制站 (PDC) 模擬器：作業執行的肯定，例如「群組原則更新密碼重設為非 AD DS 帳號，將無法正確運作。 請您繼續之前，請傳送 PDC 模擬器主角網域控制站在這個網域控制站相同的網域中的。  
  
-   架構主機：不再無法修改這個樹系的結構描述。 您繼續之前，請傳送架構主角根樹系網域中的網域控制站。  
  
-   網域命名主機：不再將能加入網域，或移除此樹系的網域。 您繼續之前，請傳送命名主角根樹系網域中的網域控制站的網域。  
  
-   這個 Active Directory 網域控制站在所有應用程式 directory 磁碟分割都將移除。 如果網域控制站保留最後一個的一或多個應用程式 directory 磁碟分割複本移除操作完成時，將不會存在的磁碟分割。  
  
請注意網域將不會存在您的網域中的最後一個網域控制站解除安裝 Active Directory Domain Services 之後。  
  
如果網域控制站是委派給管理 DNS 區域 DNS 伺服器，下列網頁將會提供移除 DNS 區域委派 DNS 伺服器的選項。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_RemovalOptions.gif)  
  
如需有關移除 AD DS，請查看[移除 Active Directory Domain Services (層級 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)和[降級網域控制站和網域和 #40;層級 200 和 #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_NewAdminPwdPage"></a>新的系統管理員密碼  
**系統管理員的新密碼**頁面會要求您提供建本機電腦的系統管理員帳號，密碼之後，請降級完成時，電腦就會網域成員伺服器或工作群組的電腦。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_NewAdminPwd.gif)  
  
如需有關移除 AD DS，請查看[移除 Active Directory Domain Services (層級 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c)和[降級網域控制站和網域和 #40;層級 200 和 #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_ConfirmRoleRemovalPage"></a>檢視選項  
**評論選項**頁面上提供您要匯出降級的組態設定 Windows PowerShell 指令碼，讓您可以將其他降級的機會。 按一下**降級**若要移除 AD DS。  
  
![AD DS 安裝](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_ReviewOptions.gif)  
  


