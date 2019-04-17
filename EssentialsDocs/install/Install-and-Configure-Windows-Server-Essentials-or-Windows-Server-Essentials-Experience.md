---
title: "安裝與設定 Windows Server Essentials 或 Windows Server Essentials 的體驗"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48ea6cd4-3955-4aaf-9236-2515a6c3e730
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8a2310b178663c6ca32a4e07d11656f1aaf2a11b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="install-and-configure-windows-server-essentials-or-windows-server-essentials-experience"></a>安裝與設定 Windows Server Essentials 或 Windows Server Essentials 的體驗

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

Windows Server Essentials 是適用於小型企業最 25 位使用者與 50 裝置理想第一次伺服器。 組織的最 100 使用者和 200 裝置，您現在可以使用 Windows Server 2012 R2 已安裝 Windows Server Essentials 體驗角色。 本主題描述這兩種案例。  
  
Windows Server Essentials 體驗是，可讓您的所有功能 （例如遠端 Web 存取和電腦備份），您可以使用在 Windows Server Essentials 鎖定而在 Windows Server Essentials 執行的限制利用 Windows Server 2016 中的角色。 此伺服器角色也在 Windows Server Essentials 有提供，而且預設會讓。
  
Windows Server Essentials 或 Essentials 體驗角色安裝之前，請注意下列限制。  
  
|在 Windows Server Essentials 的 Windows Server Essentials 體驗|Windows Server 2016 中的 Windows Server Essentials 體驗
|----|----|
|-必須位於網域控制站的樹系網域根按住必須故障。<br /><br /> -無法安裝預先存在 Active Directory 網域 （不過，還有優惠期間的執行移轉 21 日） 的環境中。|-並不一定要網域控制站已安裝現有的 Active Directory domain 的環境中。<br /><br /> -如果 Active Directory domain 不存在，安裝角色會建立 Active Directory domain，並伺服器會變成網域控制站的樹系網域根按住故障。  
|只會部署至單一網域。|只會部署至單一網域。  
|唯讀模式網域控制站不存在於您的網域。|唯讀模式網域控制站不存在於您的網域。

> [!NOTE]
>  若要下載評估版本作業系統，瀏覽 TechNet 評估中心：  
>   
>  [下載 Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)  
>   
>  [下載 Windows Server Essentials](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016-essentials)
  
## <a name="installation-options"></a>安裝選項  
 本文件會提供安裝及設定 Windows Server Essentials 的逐步指示。 根據您的網路環境，您有您可以使用下列安裝選項：  
  
-    Windows Server Essentials （與 Windows Server Essentials 體驗角色預設支援）  
  
-    Windows Server 2016 已安裝 Windows Server Essentials 體驗角色  
 
|部署環境|描述|相關一節|  
|----------------------------|-----------------|---------------------|  
|新的 Active Directory 環境|您可以安裝 Windows Server Essentials 建立新的 Active Directory 環境。|[部署 Windows Server Essentials 設定新的 Active Directory 環境](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_NewAD)|  
|現有的 Active Directory 環境|您可以安裝 Windows Server Essentials 的現有的 Active Directory 環境中。|[在現有的 Active Directory 環境中部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_ExistingAD)|  
|Virtual 環境|您可以為一樣部署 Windows Server Essentials。|[將您的環境虛擬化](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_VirtualWSE)|  
|自動的部署|您可以使用 Windows PowerShell 自動化部署的 Windows Server Essentials。|[安裝和使用 Windows PowerShell 來設定 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_PowerShell)|  
  
## <a name="before-you-begin"></a>在您開始之前  
 在您開始安裝之前，請再次檢查下列文件：  
  
-   [Windows Server Essentials Product 概觀](https://www.microsoft.com/server-cloud/windows-server-essentials/windows-server-2012-r2-essentials.aspx)  
  

-   [Windows Server Essentials 的系統需求](../get-started/system-requirements.md)   

  
##  <a name="BKMK_NewAD"></a>部署 Windows Server Essentials 設定新的 Active Directory 環境  
 Windows Server Essentials 提供讓您快速地設定 Active Directory 環境和伺服器相關的功能。  
  
###  <a name="BKMK_WSEDeploy"></a>部署 Windows Server Essentials  
 如果您使用 Windows Server Essentials，是已經支援 Windows Server Essentials 的體驗。 不過，您必須完成一些步驟來設定您的伺服器。  
  
##### <a name="to-configure-windows-server-essentials-on-a-physical-server"></a>若要設定實體伺服器上的 Windows Server Essentials  
  
1.  Windows 後**歡迎**頁面中，**設定 Windows Server Essentials 精靈**是在桌面上顯示。  
  
2.  請依照下列指示完成精靈，如下所示：  
  
    1.  在**設定 Windows Server Essentials**頁面上，按**下**。  
  
    2.  在**時間設定**，請確定您的日期、 時間和時區正確、，然後按**下**。  
  
    3.  在**公司資訊**，輸入您的公司名稱，例如**Contoso，ltd.**，然後按一下 [**下一步**。 （選擇性） 您可以變更 [伺服器名稱與內部網域名稱。  
  
    4.  在**建立網路系統管理員**，輸入新的系統管理員 account 名稱和密碼。  
  
        > [!NOTE]
        >  請勿使用預設的**系統管理員**帳號名稱和密碼。  
  
    5.  按一下**設定**。  
  
3.  伺服器便會重新開機多次在設定過程中，而且您的登入自動之前完成設定。 此程序會需要約 20 分鐘。  
  
4.  在桌面上按一下儀表板圖示以開始伺服器儀表板。 在**Home**頁面上，完成**開始**上所列的工作**設定**索引標籤。  
  
 您已完成的伺服器設定之後，將會為網域控制站設定執行 Windows Server Essentials 的伺服器。  
  
###  <a name="BKMK_DeployWSERole"></a>部署 Windows Server Essentials 的體驗中的角色 Windows Server 2012 標準 R2 和 Datacenter  
 您可以使用伺服器管理員可讓和 Windows Server 2012 標準 R2 或 Windows Server 2012 R2 Datacenter 設定 Windows Server Essentials 體驗的角色，使用下列程序。  
  
##### <a name="to-deploy-the-windows-server-essentials-experience-role-in-windows-server-2012-r2"></a>部署 Windows Server Essentials 的體驗中的角色 Windows Server 2012 R2  
  
1.  本機系統管理員身分登入您的伺服器。  
  
2.  開放**伺服器管理員**，然後按**新增角色與功能**。  
  
3.  在**選擇伺服器角色**，請選取**Windows Server Essentials 體驗**的角色。 在對話方塊中，按一下**新增功能**，然後按一下 [**下**。  
  
4.  在**功能**，按一下 [**下**。  
  
5.  檢視**Windows Server Essentials 體驗**角色描述，然後再按一下**下**。  
  
6.  在網頁，請依照下列，請按一下**下一步**，然後按一下 [確認] 頁面上**安裝**。  
  
7.  安裝完成之後，應該會伺服器角色在伺服器管理員中列出 Windows Server Essentials 的體驗。  
  
8.  旗標通知區域在伺服器管理員中，按一下旗標，然後按一下**設定 Windows Server Essentials**。  
  
9. （選擇性）視需要變更伺服器名稱。  
  
    > [!IMPORTANT]
    >  設定 Windows Server Essentials 之後，您無法變更伺服器名稱。  
  

10. 依照精靈中之前所述設定 Windows Server Essentials[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)一節。  

10. 依照精靈中之前所述設定 Windows Server Essentials[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)一節。  

  
##  <a name="BKMK_ExistingAD"></a>在現有的 Active Directory 環境中部署 Windows Server Essentials  
 如果您的組織已經有現有 Active Directory 環境，您也可以部署 Windows Server Essentials。 此外，您可以選擇您要部署為網域控制站的 Windows Server Essentials。  
  
> [!IMPORTANT]
>  此選項才可以使用 Windows Server Essentials 的體驗中的角色 Windows Server 2012 標準 R2 或 Windows Server 2012 R2 資料中心部署。  
  
#### <a name="to-deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a>若要在現有的 Active Directory 環境中部署 Windows Server Essentials  
  
1.  （選擇性）視需要變更伺服器名稱。  
  
    > [!IMPORTANT]
    >  設定 Windows Server Essentials 之後，您無法變更伺服器名稱。  
  
2.  加入您的伺服器，如下所示現有網域執行 Windows Server Essentials:  
  
    1.  如果您希望此為網域控制站伺服器，請為網域控制站複本設定伺服器。  
  
    2.  如果您不想此為網域控制站伺服器，使用 Windows 原生工具此伺服器加入至您的網域。  
  
3.  重新播放您的伺服器，並伺服器網域系統管理員身分登入。  
  
4.  打開伺服器管理員]，然後按一下**新增角色與功能**。  
  
5.  頁面，請依照下列，請按一下**下一步**。  
  
6.  在**選取伺服器角色**、 **Windows Server Essentials 體驗**。 在對話方塊中，按一下**新增功能**，然後按一下 [**下**。  
  
7.  在**功能**，按一下 [**下**。  
  
8.  檢視**Windows Server Essentials 體驗**描述，然後再按一下**下**。  
  
9. 在網頁，請依照下列，請按一下**下一步**，然後按一下 [確認] 頁面上**安裝**。  
  
10. 安裝完成後，Windows Server Essentials 體驗會列為伺服器角色在伺服器管理員中。  
  
11. 在旗標通知區域中**伺服器管理員**、 [旗標，然後**設定 Windows Server Essentials**。  
  
12. 請依照下列設定 Windows Server Essentials 精靈。 根據您的 Active Directory 設定，您會收到是否您設定 Windows Server Essentials 的網域控制站或網域成員。 按一下**設定**若要開始設定。 設定程序會需要約 10 分鐘的時間來完成。  
  
##  <a name="BKMK_VirtualWSE"></a>將您的環境虛擬化  
  Windows Server Essentials、 Windows Server 2012 R2 標準，以及 Windows Server 2012 R2 Datacenter 可以為電腦虛擬執行。 您在執行 HYPER-V server 使用 HYPER-V 管理工具來執行虛擬電腦。 Windows Server Essentials 授權觀點，可讓您設定 HYPER-V 角色與虛擬化您的環境。 授權可讓您執行的 Windows Server Essentials 的另一個來賓作業系統系統設定。 根據您的系統提供者] 度的設定，Windows Server Essentials 可讓您設定模擬環境順暢地進行。  
  
#### <a name="to-deploy-windows-server-essentials-as-a-virtual-machine"></a>Windows Server Essentials 部署一樣為  
  
1.  （根據您的系統提供者] 度的設定），在 Windows 歡迎畫面後**在您開始之前**頁面提供的選項，來設定 Windows Server Essentials 實體硬體或做為 virtual 執行個體。 系統提供者預先定義的可用性這些選項，這兩個選項可隨時無法使用。 在安裝 Windows Server Essentials 一樣，以**安裝 Windows Server Essentials**，請選取**安裝 virtual 執行個體為**，，然後按一下 [**設定**。  
  
2.  精靈將會提供一樣，需要約 5 分鐘的時間。  
  

3.  接下來，設定 Windows Server Essentials 稍早中所述[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)一節。  

3.  接下來，設定 Windows Server Essentials 稍早中所述[部署 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy)一節。  

  
##  <a name="BKMK_PowerShell"></a>安裝和使用 Windows PowerShell 來設定 Windows Server Essentials  
 您可以使用 Windows PowerShell cmdlet 自動化 Windows Server Essentials 的安裝。  
  
#### <a name="to-install-windows-server-essentials-by-using-windows-powershell"></a>使用 Windows PowerShell 來安裝 Windows Server Essentials  
  
1.  從提升權限的命令提示字元開放的 Windows PowerShell 主機。  
  
2.  使用下列命令來安裝 Windows Server Essentials 體驗的角色：  
  
    ```  
    Add-WindowsFeature ServerEssentialsRole  
    ```  
  
3.  執行`Get-Help Start-WssConfigurationService`。  
  
    1.  執行下列命令，[開始] 設定為網域控制站的 Windows Server Essentials 的設定：  
  
        ```  
        Start-WssConfigurationService -CompanyName "ContosoTest" -DNSName "ContosoTest.com" -NetBiosName "ContosoTest" -ComputerName "YourServerName  œNewAdminCredential $cred  
        ```  
  
    2.  執行下列命令，[開始] 設定為現有網域成員 Windows Server Essentials 的設定。 您必須執行這項工作管理員企業群組和 Active Directory Domain 管理員群組成員：  
  
        ```  
        Start-WssConfigurationService  œCredential <Your Credential>  
  
        ```  
  
4.  檢視安裝的進度使用下列命令：  
  
    -   若要安裝的進度列上顯示的狀態，請執行`Get-WssConfigurationStatus  œShowProgress`。  
  
    -   若要立即進度而不需要的進度列，請執行`Get-WssConfigurationStatus`。  
  
## <a name="see-also"></a>也了  
  
-   [Windows Server Essentials 中的新功能](../get-started/what-s-new.md)  
  
-   [安裝 Windows Server Essentials](Install-Windows-Server-Essentials.md)  
  
-   [開始使用 Windows Server Essentials](../get-started/get-started.md)
