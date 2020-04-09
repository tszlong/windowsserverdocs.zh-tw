---
title: 安裝和設定 Windows Server Essentials 或 Windows Server Essentials 體驗
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 48ea6cd4-3955-4aaf-9236-2515a6c3e730
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2605483de447e78fc1bd091d911867656c8ab8b6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820044"
---
# <a name="install-and-configure-windows-server-essentials-or-windows-server-essentials-experience"></a>安裝和設定 Windows Server Essentials 或 Windows Server Essentials 體驗

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

對於擁有多達25位使用者和50裝置的小型企業而言，Windows Server Essentials 是理想的第一部伺服器。 對於擁有多達100使用者和200裝置的組織，您現在可以使用已安裝 Windows Server Essentials 體驗角色的 Windows Server 2012 R2。 本主題描述這兩種案例。  
  
Windows Server Essentials 體驗是 Windows Server 2016 中的一項角色，可讓您利用 Windows Server Essentials 中可供您使用的所有功能（例如遠端 Web 存取和 PC 備份），而不會有在中強制執行的鎖定和限制 Windows Server Essentials。 Windows Server Essentials 也提供此伺服器角色，且預設為啟用。
  
在您安裝 Windows Server Essentials 或 Essentials 體驗角色之前，請注意下列限制。  
  
|Windows Server essentials 中的 windows Server Essentials 體驗|Windows Server 2016 中的 windows Server Essentials 體驗
|----|----|
|-必須是位於樹系和網域根目錄的網域控制站，而且必須保留所有 FSMO 角色。<br /><br /> -無法安裝在具有既有 Active Directory 網域的環境中（不過，有21天的寬限期可執行遷移）。|-如果是安裝在具有既有 Active Directory 網域的環境中，則不一定要是網域控制站。<br /><br /> -如果 Active Directory 網域不存在，安裝角色將會建立 Active Directory 網域，而伺服器會成為樹系和網域根目錄的網域控制站，並保留所有 FSMO 角色。  
|只能部署到單一網域。|只能部署到單一網域。  
|您的網域中不能有唯讀網域控制站。|您的網域中不能有唯讀網域控制站。

> [!NOTE]
>  若要下載評估版本的作業系統，請造訪 TechNet 評估中心：  
>   
>  [下載 Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)  
>   
>  [下載 Windows Server Essentials](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016-essentials)
  
## <a name="installation-options"></a>安裝選項  
 本文件提供安裝與設定 Windows Server Essentials 的逐步指示。 根據您的網路環境，您會有下列安裝選項：  
  
-    Windows Server Essentials （預設已啟用 Windows Server Essentials 體驗角色）  
  
-    已安裝 Windows Server Essentials 體驗角色的 windows Server 2016  
 
|部署環境|描述|相關章節|  
|----------------------------|-----------------|---------------------|  
|新的 Active Directory 環境|您可以安裝 Windows Server Essentials 來建立新的 Active Directory 環境。|[部署 Windows Server Essentials 以設定新的 Active Directory 環境](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_NewAD)|  
|現有的 Active Directory 環境|您可以在現有的 Active Directory 環境中安裝 Windows Server Essentials。|[在現有的 Active Directory 環境中部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_ExistingAD)|  
|虛擬環境|您可以部署 Windows Server Essentials 為虛擬機器。|[虛擬化您的環境](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_VirtualWSE)|  
|自動化部署|您可以使用 Windows PowerShell 來自動化部署 Windows Server Essentials。|[使用 Windows PowerShell 安裝和設定 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_PowerShell)|  
  
## <a name="before-you-begin"></a>開始之前  
 在開始安裝之前，請先檢閱下列文件：  
  
-   [Windows Server Essentials 產品總覽](https://www.microsoft.com/server-cloud/windows-server-essentials/windows-server-2012-r2-essentials.aspx)  
  

-   [Windows Server Essentials 系統需求](../get-started/system-requirements.md)   

  
##  <a name="deploy-windows-server-essentials-to-set-up-a-new-active-directory-environment"></a><a name="BKMK_NewAD"></a>部署 Windows Server Essentials 以設定新的 Active Directory 環境  
 Windows Server Essentials 可讓您快速設定 Active Directory 環境及相關的伺服器功能。  
  
###  <a name="deploying-windows-server-essentials"></a><a name="BKMK_WSEDeploy"></a>部署 Windows Server Essentials  
 如果您使用 Windows Server Essentials，則已啟用 Windows Server Essentials 體驗。 不過，您必須先完成某些步驟，才能設定您的伺服器。  
  
##### <a name="to-configure-windows-server-essentials-on-a-physical-server"></a>在實體伺服器上設定 Windows Server Essentials  
  
1. 在 Windows [歡迎] 頁面之後，您在桌面上會看到 [設定 Windows Server Essentials 精靈]。  
  
2. 請遵循下列指示完成精靈：  
  
   1.  在 [設定 Windows Server Essentials] 頁面上，按 [下一步]。  
  
   2.  在 [時間設定] 中，確認您的日期、時間和時區是正確的，然後按 [下一步]。  
  
   3.  在 [公司資訊] **中，輸入您的公司名稱，如**Contoso,Ltd. **，然後按 [下一步]** 。 (選擇性) 您可以變更內部網域名稱和伺服器名稱。  
  
   4.  在 [建立網路系統管理員] 中，輸入新的系統管理員帳戶名稱和密碼。  
  
       > [!NOTE]
       >  不要使用預設的 [系統管理員] 帳戶名稱和密碼。  
  
   5.  按一下 **[設定]** 。  
  
3. 伺服器會在設定過程中重新啟動多次，設定完成後，就會自動將您登入。 此過程大概會花費 20 分鐘的時間。  
  
4. 在桌面上，按一下儀表板圖示以啟動伺服器儀表板。 在 [首頁] 頁面上，完成 [設定] 索引標籤上所列的 [快速入門] 工作。  
  
   完成伺服器設定之後，執行 Windows Server Essentials 的伺服器會設定為網域控制站。  
  
###  <a name="deploying-the-windows-server-essentials-experience-role-in-windows-server-2012-r2-standard-and-datacenter"></a><a name="BKMK_DeployWSERole"></a>在 Windows Server 2012 R2 Standard 和 Datacenter 中部署 Windows Server Essentials 體驗角色  
 您可以使用伺服器管理員，在 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 中啟用和設定 Windows Server Essentials 體驗角色，方法是使用下列程式。  
  
##### <a name="to-deploy-the-windows-server-essentials-experience-role-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 中部署 Windows Server Essentials 體驗角色  
  
1.  以本機系統管理員身分登入您的伺服器。  
  
2.  開啟 [伺服器管理員]，然後按一下 [新增角色及功能]。  
  
3.  在 [選取伺服器角色] 中，選取 [Windows Server Essentials 體驗] 角色。 在對話方塊中，按一下 [新增功能]，然後按 [下一步]。  
  
4.  在 [功能] 中，按 [下一步]。  
  
5.  檢閱 [Windows Server Essentials 體驗] 角色描述，然後按 [下一步]。  
  
6.  在接下來的頁面中，按 [下一步]，然後在確認頁面中按一下 [安裝]。  
  
7.  安裝完成之後，Windows Server Essentials 體驗應該會在伺服器管理員中列為伺服器角色。  
  
8.  在伺服器管理員中的旗標通知區域按一下旗標，然後按一下 [設定 Windows Server Essentials]。  
  
9. (選擇性) 如果需要，變更伺服器名稱。  
  
    > [!IMPORTANT]
    >  設定 Windows Server Essentials 之後，就無法再變更伺服器名稱。  
  

10. 依照先前的[部署 Windows Server essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)一節中所述，遵循嚮導來設定 Windows server essentials。  

10. 依照先前的[部署 Windows Server essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)一節中所述，遵循嚮導來設定 Windows server essentials。  

  
##  <a name="deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a><a name="BKMK_ExistingAD"></a>在現有的 Active Directory 環境中部署 Windows Server Essentials  
 如果您的組織已經有現有的 Active Directory 環境，您也可以部署 Windows Server Essentials。 此外，您還可以選擇是否要部署 Windows Server Essentials 為網域控制站。  
  
> [!IMPORTANT]
>  只有當您在 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 中部署 Windows Server Essentials 體驗角色時，才可以使用此選項。  
  
#### <a name="to-deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a>在現有的 Active Directory 環境中部署 Windows Server Essentials  
  
1.  (選擇性) 如果需要，變更伺服器名稱。  
  
    > [!IMPORTANT]
    >  設定 Windows Server Essentials 之後，就無法再變更伺服器名稱。  
  
2.  將您執行 Windows Server Essentials 的伺服器加入現有的網域，如下所示：  
  
    1.  如果您想要讓此伺服器成為網域控制站，請將此伺服器設定為複本網域控制站。  
  
    2.  如果不想使用此伺服器做為網域控制站，請使用 Windows 原生工具將此伺服器加入您的網域。  
  
3.  重新啟動您的伺服器，並以網域系統管理員身分登入伺服器。  
  
4.  開啟 [伺服器管理員]，然後按一下 [新增角色及功能]。  
  
5.  在接下來的頁面中，按 [下一步]。  
  
6.  在 [選取伺服器角色] 中，選取 [Windows Server Essentials 體驗] 角色。 在對話方塊中，按一下 [新增功能]，然後按 [下一步]。  
  
7.  在 [功能] 中，按 [下一步]。  
  
8.  檢閱 [Windows Server Essentials 體驗] 描述，然後按 [下一步]。  
  
9. 在接下來的頁面中，按 [下一步]，然後在確認頁面中按一下 [安裝]。  
  
10. 安裝完成之後，Windows Server Essentials 體驗會在伺服器管理員中列為伺服器角色。  
  
11. 在 [伺服器管理員] 的旗標通知區域中，按一下旗標，然後按一下 [設定 Windows Server Essentials]。  
  
12. 依照精靈設定 Windows Server Essentials。 根據您的 Active Directory 組態，您會在網域控制站上設定 Windows Server Essentials 或做為網域成員。 按一下 [設定] 開始設定。 設定過程需要花費大約 10 分鐘的時間。  
  
##  <a name="virtualize-your-environment"></a><a name="BKMK_VirtualWSE"></a>虛擬化您的環境  
  Windows Server Essentials、Windows Server 2012 R2 Standard 和 Windows Server 2012 R2 Datacenter 可以做為虛擬機器來執行。 您可以在執行 HYPER-V 的伺服器上，使用 HYPER-V 管理工具來執行虛擬機器。 從授權的觀點來看，Windows Server Essentials 可讓您設定 Hyper-v 角色並虛擬化您的環境。 授權可讓您設定另一個正在執行 Windows Server Essentials 的客體作業系統。 Windows Server Essentials 可讓您順暢地設定虛擬化環境，這取決於您的系統提供者「存儲設置」。  
  
#### <a name="to-deploy-windows-server-essentials-as-a-virtual-machine"></a>部署 Windows Server Essentials 為虛擬機器。  
  
1.  在 Windows 歡迎頁面（取決於您的系統提供者「設定」）之後，[在**您開始前**] 頁面會提供選項，將 Windows Server Essentials 設定為虛擬實例或在實體硬體上。 這些選項是否可用由您的系統提供者預先定義，這兩個選項都可能永遠無法使用。 若要將 Windows Server Essentials 安裝為虛擬機器，請在 [**安裝 Windows Server essentials**] 中選取 [**安裝為虛擬實例**]，然後按一下 [**設定**]。  
  
2.  精靈將花費大約五分鐘的時間佈建虛擬機器。  
  

3.  接下來，設定 Windows Server Essentials，如先前的[部署 Windows Server essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)一節中所述。  

3.  接下來，設定 Windows Server Essentials，如先前的[部署 Windows Server essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)一節中所述。  

  
##  <a name="install-and-configure-windows-server-essentials-by-using-windows-powershell"></a><a name="BKMK_PowerShell"></a>使用 Windows PowerShell 安裝和設定 Windows Server Essentials  
 您可以使用 Windows PowerShell Cmdlet 來自動化安裝 Windows Server Essentials。  
  
#### <a name="to-install-windows-server-essentials-by-using-windows-powershell"></a>使用 Windows PowerShell 安裝 Windows Server Essentials  
  
1.  從提升權限的命令提示字元開啟 Windows PowerShell 主控台。  
  
2.  使用下列命令來安裝 Windows Server Essentials 體驗角色：  
  
    ```  
    Add-WindowsFeature ServerEssentialsRole  
    ```  
  
3.  執行 `Get-Help Start-WssConfigurationService`。  
  
    1.  執行下列命令以啟動的組態來設定 Windows Server Essentials 為網域控制站：  
  
        ```  
        Start-WssConfigurationService -CompanyName "ContosoTest" -DNSName "ContosoTest.com" -NetBiosName "ContosoTest" -ComputerName "YourServerName  œNewAdminCredential $cred  
        ```  
  
    2.  執行下列命令以啟動組態來設定 Windows Server Essentials 為現有網域成員： 您必須是 Enterprise Admin 群組和 Active Directory 中 Domain Admin 群組的成員，才能執行這項工作：  
  
        ```  
        Start-WssConfigurationService  œCredential <Your Credential>  
  
        ```  
  
4.  使用下列命令監控安裝進度：  
  
    -   若要在進度列上顯示安裝狀態，請執行 `Get-WssConfigurationStatus  œShowProgress`。  
  
    -   若要直接了解即時進度而不要進度列，請執行 `Get-WssConfigurationStatus`。  
  
## <a name="see-also"></a>另請參閱  
  
-   [Windows Server Essentials 的新功能](../get-started/what-s-new.md)  
  
-   [安裝 Windows Server Essentials](Install-Windows-Server-Essentials.md)  
  
-   [Windows Server Essentials 入門](../get-started/get-started.md)
