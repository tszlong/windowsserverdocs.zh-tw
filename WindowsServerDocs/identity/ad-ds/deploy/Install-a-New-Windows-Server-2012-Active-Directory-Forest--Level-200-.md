---
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: "安裝新 Windows Server 2012 Active Directory 森林 (層級 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b8a7502a1b9d27b0f61353f2544182a64d311496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>安裝新 Windows Server 2012 Active Directory 森林 (層級 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題解釋的新 Windows Server 2012 Active Directory Domain Services 網域控制站促銷功能簡介層級。 在 Windows Server 2012，AD DS 會取代帶領工具與伺服器管理員及 Windows PowerShell 為基礎的部署系統。  
  
-   [Active Directory Domain 服務簡化的管理](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)  
  
-   [技術概觀](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)  
  
-   [部署樹系的伺服器管理員](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [部署 Windows PowerShell 中的樹系](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)  
  
## <a name="BKMK_SimplifiedAdmin"></a>Active Directory Domain 服務簡化的管理  
Windows Server 2012 導入下一代 Active Directory Domain 服務簡化管理的而且最根本網域重新構想自 Windows 2000 Server。 AD DS 簡化管理拍下全家 Active directory 12 年來，並讓更多可支援、更具彈性、更直覺管理體驗 architects 和系統管理員。 這是建立現有的技術，以及擴充功能的元件在 Windows Server 2008 R2 推出的最新版本。  
  
### <a name="what-is-ad-ds-simplified-administration"></a>何謂 AD DS 簡化管理嗎？  
AD DS 簡化管理是 reimagining 網域部署。 這些功能包括：  
  
-   AD DS 角色部署現在伺服器管理員架構新的一部分，並且可讓遠端安裝。  
  
-   AD DS 部署和設定引擎時，現在 Windows PowerShell，甚至使用的圖形安裝。  
  
-   立即升級包括必要條件檢查驗證樹系和網域整備新的網域控制站，降低失敗促銷活動的機會。  
  
-   Windows Server 2012 樹系層級尚未實作新功能和網域功能等級為僅針對新 Kerberos 功能，減輕常用的系統管理員子集需要的功能需要質網域控制站環境。  
  
### <a name="purpose-and-benefits"></a>用途與優點  
這些變更可能會更加複雜，不簡單。 但重新設計 AD DS 部署程序，有是機會到較少、 變得更容易動作聯合許多步驟與最佳做法的規範。 這表示，例如，新的複本網域控制站的圖形設定現在是 8 對話方塊，而非之前的 12。 建立新的 Active Directory 樹系需要*單一*僅限使用 Windows PowerShell 命令*一*引數： 的網域名稱。  
  
有為何這類重點放在 Windows Server 2012 中的 Windows PowerShell？ 隨著分散式運算發展，Windows PowerShell 可設定和維護圖形從命令列介面的單一引擎。 它可以讓完整功能指令碼的任何具有相同一流表現元件適用於 IT 專業人員的 API 會授與開發人員。 隨著普遍運算以雲端為基礎，Windows PowerShell 也最後帶來的能力的遠端管理的伺服器，其中不圖形介面的電腦具有相同的管理功能當成一個使用的監視器和滑鼠。  
  
資深 AD DS 系統管理員應該會看到他們的上一個知識高度相關。 開始系統管理員可以找到遠淺學習。  
  
## <a name="BKMK_TechOverview"></a>技術概觀  
  
### <a name="what-you-should-know-before-you-begin"></a>有哪些您應該要知道您開始之前  
本主題假設熟悉的 Active Directory Domain Services，舊版並不提供其用途與功能的基礎詳細資料。 如需 AD DS，查看 TechNet 入口網站連結下方頁面：  
  
-   [Windows Server 2008 R2 的 active Directory Domain Services](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
  
-   [Windows Server 2008 的 active Directory Domain Services](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
  
-   [Windows Server Technical 參考資料](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
  
### <a name="functional-descriptions"></a>描述的功能  
  
#### <a name="ad-ds-role-installation"></a>安裝 AD DS 角色  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)  
  
Active Directory Domain Services 安裝會使用伺服器管理員及 Windows PowerShell，例如所有其他伺服器角色與 Windows Server 2012 中的功能。 Dcpromo.exe 程式不會再提供 GUI 設定選項。  
  
在本機和遠端安裝在伺服器管理員或 ServerManager 模組圖形精靈用於 Windows PowerShell 中。 來執行多個那些精靈或 cmdlet 執行個體目標不同的伺服器，您可以將部署 AD DS 多網域控制站同時，全都透過單一的單一主機。 雖然這些新功能不是回溯相容性與 Windows Server 2008 R2 或更早版本作業系統，您也仍然可以使用 Dism.exe 應用程式的命令列傳統本機角色安裝在 Windows Server 2008 R2 推出。  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)  
  
#### <a name="ad-ds-role-configuration"></a>AD DS 角色設定  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
Active Directory Domain Services 設定 「 先前稱為帶領 」 是目前的角色安裝所謂作業。 安裝之後 AD DS 角色，系統管理員可以設定伺服器為網域控制站使用不同的精靈在伺服器管理員中，或使用 ADDSDeployment Windows PowerShell 模組。  
  
AD DS 角色設定欄位體驗中的 12 個年的組建，並現在設定網域控制站根據最新 Microsoft 最佳做法的規範。 例如，網域名稱系統 」 和 「 通用目錄安裝每個網域控制站預設。  
  
伺服器管理員 AD DS 設定精靈將許多個人對話方塊合併到較少的提示，並不會再隱藏在 [進階] 模式中的設定。 在安裝期間，其中一個展開對話方塊中是整個升級程序。 精靈，並 ADDSDeployment Windows PowerShell 模組顯示值得注意的變更並安全性考量，連結的詳細資訊。  
  
Dcpromo.exe 僅命令列自動安裝會留在 Windows Server 2012，並不會再執行圖形安裝精靈中。 我們建議您停止使用的自動安裝 Dcpromo.exe 並 ADDSDeployment 模組，以取代為現在取代可執行檔將不會包含在下一個版本的 Windows。  
  
這些新功能不是以 Windows Server 2008 R2 或較舊的作業系統回溯相容性。  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)  
  
> [!IMPORTANT]  
> Dcpromo.exe 不再包含圖形精靈，並不會再安裝二進位角色或功能。 請嘗試執行 Explorer 殼層傳回 Dcpromo.exe:  
>   
> 「 Active Directory Domain Services 安裝精靈搬在伺服器管理員中。 如需詳細資訊，請 https://go.microsoft.com/fwlink/?LinkId=220921。」  
>   
> 嘗試執行 Dcpromo.exe / 自動仍然安裝二進位檔，如下所示先前的作業系統，但會警告：  
>   
> 「 帶領自動的操作取代的 Windows PowerShell 模組 ADDSDeployment。 如需詳細資訊，請 https://go.microsoft.com/fwlink/?LinkId=220924。」  
>   
> Windows Server 2012 deprecates dcpromo.exe 並不會包含在未來的 Windows 版本也將它收到關於調節此作業系統。 系統管理員應該停止使用，若要建立網域控制站從命令列切換到支援的 Windows PowerShell 模組。  
  
#### <a name="prerequisite-checking"></a>必要條件檢查  
網域控制站設定也會實作評估的樹系和網域繼續網域控制站升級之前的必要條件檢查階段。 這包括 FSMO 角色可用性、 使用者權限、 延伸的架構相容性及其他需求。 這個新的設計可以減輕位置網域控制站升級開始，然後中止中途島組態嚴重錯誤的問題。 這會失去關聯的網域控制站中繼資料森林中的機會減少或不正確認為這伺服器網域控制站。  
  
## <a name="BKMK_SMForest"></a>部署樹系的伺服器管理員  
本章節如何安裝的第一個網域控制站森林根網域圖形 Windows Server 2012 電腦上使用伺服器管理員中。  
  
### <a name="server-manager-ad-ds-role-installation-process"></a>伺服器管理員 AD DS 角色安裝程序  
下圖顯示 Active Directory Domain Services 角色安裝程序，開頭為您執行 ServerManager.exe 和結束網域控制站在升級之前的權限。  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)  
  
#### <a name="server-pool-and-add-roles"></a>伺服器集區與新增角色  
從執行伺服器管理員可存取的任何 Windows Server 2012 電腦的符合資格的共用。 一旦共用，您選取這些伺服器遠端安裝 AD DS，或在伺服器管理員中可能任何其他設定選項。  
  
若要新增的伺服器，請選擇下列其中一個動作：  
  
-   按一下**以管理新增其他伺服器**在儀表板歡迎畫面的磚  
  
-   按一下**管理**功能表，然後選取**新增伺服器**  
  
-   以滑鼠右鍵按一下**所有伺服器]** ，然後選擇 [**新增伺服器**  
  
這時新增伺服器對話方塊：  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)  
  
這可讓您使用或群組集區中新增伺服器三種方式：  
  
-   Active Directory 搜尋使用 LDAP (需要電腦所屬的網域、 作業系統篩選可讓和支援萬用字元）  
  
-   DNS 搜尋 （使用 DNS 別名或 ARP 或 NetBIOS 廣播或 WINS 對應，透過 IP 位址不允許作業系統篩選或支援萬用字元）  
  
-   匯入 （使用伺服器分隔 CR 日 LF 文字檔案清單）  
  
按一下**現在尋找**返回從該相同的 Active Directory 網域的電腦已經加入的伺服器清單，按一下 [一或多個伺服器名稱，從清單中的伺服器。 按一下 [新增至伺服器向**選取**清單中。 使用**新增伺服器]**對話方塊中選取的伺服器新增至儀表板角色群組。 或按一下 [**管理**，，然後按一下 [**建立伺服器群組**，或按一下 [**建立伺服器群組**儀表板上**歡迎伺服器管理員**來建立群組自訂伺服器] 磚。  
  
> [!NOTE]  
> 新增伺服器程序未驗證伺服器是 online 或無障礙。 不過，無法存取的任何伺服器旗標管理檢視在伺服器管理員中下, 一步重新整理中  
  
您可以安裝角色遠端任何的 Windows Server 2012 上的電腦新增集區，所示：  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)  
  
您無法完全管理執行 Windows Server 2012 較舊的作業系統的伺服器。 **新增角色與功能**選取項目執行的 ServerManager Windows PowerShell 模組**安裝-WindowsFeature**。  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)  
  
您也可以使用現有的網域控制站伺服器管理員儀表板，以選取遠端伺服器 AD DS 安裝角色已經預先選取 AD DS 儀表板磚上按一下滑鼠右鍵，然後選取**AD DS 新增另一部伺服器以**。 這會叫用**安裝-WindowsFeature AD 網域服務**。  
  
在電腦執行伺服器管理員集區，本身自動。 若要安裝的 AD DS 角色時，只要按一下**管理**功能表和**新增角色與功能**。  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)  
  
#### <a name="installation-type"></a>安裝類型  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)  
  
**安裝類型**對話方塊中提供的選項，不支援 Active Directory Domain Services:**遠端桌面服務案例-安裝**。 這個選項只會在多部伺服器分散式工作負載允許遠端桌面服務。 如果您選取它，無法安裝 AD DS。  
  
隨時安裝 AD DS 保留就地預設選項：**安裝以角色為基礎，或為基礎的功能的**。  
  
#### <a name="server-selection"></a>伺服器選取項目  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)  
  
**選擇伺服器**對話方塊，可讓您選擇其中一集區之前加入伺服器，只要無障礙。 本機伺服器執行伺服器管理員是可供使用。  
  
此外，您可以選擇與 Windows Server 2012 作業系統 HYPER-V VHD 生效和伺服器管理員加入角色它們直接透過服務的元件。 這可讓您提供的必要元件 virtual 伺服器才能進一步進行設定。  
  
#### <a name="server-roles-and-features"></a>伺服器角色與功能  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)  
  
選取 [ **Active Directory Domain Services**如果您想要升級網域控制站的角色。 所有 Active Directory 的管理功能和服務需要自動安裝更新，即使它們是很明顯是另一個角色或不會顯示已選取在伺服器管理員介面。  
  
伺服器管理員也會顯示此角色隱含安裝; 的管理功能的資訊] 對話方塊這是相當於**-IncludeManagementTools**引數。  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)  
  
其他**功能**可以在此處加入像您想要。  
  
#### <a name="active-directory-domain-services"></a>Active Directory Domain Services  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)  
  
**Active Directory Domain Services**對話方塊需求與最佳做法提供有限的資訊。 確認您選擇 AD DS 角色為主要做 」 這個畫面未顯示，如果您未選取 AD DS。  
  
#### <a name="confirmation"></a>確認  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)  
  
**確認**對話方塊是檢查最後一個點之前的角色安裝開始。 它所提供的選項開機視角色安裝之後，但 AD DS 安裝並不需要重新開機。  
  
按一下**安裝**，即可開始安裝角色您確認。 開始後，您就無法取消安裝角色。  
  
#### <a name="results"></a>結果  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)  
  
**結果**對話方塊中顯示目前安裝進度和目前安裝的狀態。 安裝角色持續無論是否伺服器管理員已關閉。  
  
安裝結果驗證仍是最好的作法。 如果您關閉**結果**對話方塊安裝完成之前，您可以檢查伺服器管理員通知旗標結果。 伺服器管理員也會顯示一則警告訊息的任何已安裝 AD DS 角色但進一步並未設定為網域控制站伺服器。  
  
**工作的通知**  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)  
  
**AD DS 詳細資料**  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)  
  
**工作的詳細資料**  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)  
  
#### <a name="promote-to-domain-controller"></a>升級為網域控制站  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)  
  
結尾的角色安裝 AD DS，您可以繼續進行設定使用**這個網域控制站伺服器升級**連結。 這必要伺服器網域控制站，但不一定要立即執行設定精靈。 例如您可能只想要提供使用的 AD DS 二進位檔案伺服器之前將它們傳送到另一個分公司較新的設定。 透過新增 AD DS 角色交貨之前，您儲存當到達目的地的時間。 您也可以依照不天或星期保持 offline 網域控制站的最佳做法。 最後，這可讓您更新元件之前網域控制站升級，儲存您的後續重新開機至少一次。  
  
選取此連結稍後會叫用 ADDSDeployment cmdlet:**安裝-addsforest**，**安裝-addsdomain**，或**安裝-addsdomaincontroller**。  
  
### <a name="uninstallingdisabling"></a>解除安裝/停用  
移除 AD DS 角色像任何其他的角色，無論您是否升級為網域控制站伺服器。 不過，移除 AD DS 角色需要重新開機完成。  
  
Active Directory Domain Services 角色移除點不同安裝，在它需要網域控制站降級，才能完成。 這是必要以避免網域控制站其解除安裝，而不適當的中繼資料清除森林中的角色二進位檔。 如需詳細資訊，請查看[降級網域控制站和網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
> [!WARNING]  
> 升級為網域控制站不支援後將會防止伺服器通常會開機，請移除 Dism.exe 或 Windows PowerShell DISM 模組 AD DS 角色。  
>   
> 伺服器管理員與或不同的 Windows PowerShell 模組 AD DS 部署，DISM 是原生維護系統有既有不知道 AD DS 或其設定。 請勿使用 Dism.exe 或 Windows PowerShell DISM 模組除非伺服器不再網域控制站解除安裝 AD DS 角色。  
  
### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>建立 AD DS 森林根網域與伺服器管理員  
下圖顯示 Active Directory Domain Services 設定程序，如此，您先前安裝 AD DS 角色並開始在**Active Directory Domain Services 組態精靈**使用伺服器管理員。  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)  
  
#### <a name="deployment-configuration"></a>部署設定  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)  
  
伺服器管理員會開始使用每個網域控制站升級**部署組態**頁面。 剩餘的選項與所需的欄位變更此頁面上，後續的部署操作根據您選擇的頁面。  
  
若要建立新的 Active Directory 森林，請按一下**新增新的樹系**。 您必須提供有效的根網域名稱。名稱無法單一標示 (必須名稱，例如*contoso.com*或類似和不只是*以 contoso*) 必須使用允許的 DNS 網域命名需求。  
  
如需有關有效的網域名稱，查看知識庫文章[適用於電腦、 網域、 網站及 Ou 命名 Active Directory 規格](https://support.microsoft.com/kb/909264)。  
  
> [!WARNING]  
> 無法建立新的 Active Directory 樹系的外部 DNS 名稱相同的名稱。 例如 http://contoso.com DNS URL 網際網路時，您必須選擇不同的名稱為內部樹系避免未來的相容性問題。 該名稱應該唯一和網路流量的。 例如： corp.contoso.com。  
  
新的樹系的網域中的系統管理員 account 就不需要新的認證。 網域控制站升級程序使用來自第一次用來建立樹系根的網域控制站建的認證。 就不 （預設） 來停用或鎖定建，可能會樹系的唯一的進入點的系統管理網域帳號會進入不穩定。 請務必部署新的樹系之前必須知道的密碼。  
  
**網域名稱**需要有效的完整的網域 DNS 名稱，就需要。  
  
#### <a name="domain-controller-options"></a>網域控制站選項  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)  
  
**網域控制站選項**可讓您設定**樹系功能等級**並**網域功能等級**新的樹系根網域。 根據預設，這些設定的新的樹系根網域中的 Windows Server 2012。 Windows Server 2012 的樹系功能等級透過 Windows Server 2008 R2 的樹系功能等級不提供任何新的功能。 只為了實作新 Kerberos 設定所需的 Windows Server 2012 網域功能等級 [永遠提供宣告 」 和 「 失敗護身的驗證要求 」。 Windows Server 2012 中功能層級的主要使用是限制需求允許的最小作業系統的網域控制站參與。 亦即，您可以指定 Windows Server 2012 網域功能層級只網域控制站執行 Windows Server 2012 可以裝載網域。  Windows Server 2012 實作新的網域控制站標幟稱為**DS_WIN8_REQUIRED**在**DSGetDcName**功能的專屬找出 Windows Server 2012 網域控制站的 NetLogon。 這可讓您的網域控制站在執行作業系統的允許，則多同或異質性樹系彈性。  
  
如需網域控制站的位置，檢視[Directory 服務功能](https://msdn.microsoft.com/library/ms675900(VS.85).aspx)。  
  
僅限可設定的網域控制站功能是 [DNS 伺服器] 選項。 Microsoft 建議所有網域控制站都提供 DNS 服務的可用性分散式的環境中，這是安裝任何模式或網域中的網域控制站預設選取此選項的原因。 通用和朗讀只網域控制站選項時，會無法使用建立新的樹系根網域。第一次網域控制站必須 GC、，且無法讀取只有網域控制站 (RODC)。  
  
指定**Directory 服務還原模式密碼**必須遵守密碼原則套用到伺服器，預設不需要穩固密碼。僅限非空白一個。 隨時複雜的密碼或最好複雜密碼。  
  
#### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 選項]，然後 DNS 的認證委派  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)  
  
**DNS 選項**頁面上可讓您設定 DNS 委派，並提供其他 DNS 系統管理員認證。  
  
您無法設定 DNS 選項或委派 Active Directory Domain Services 組態精靈中安裝新 Active Directory 森林根網域何處選取**的 DNS 伺服器**在**網域控制站選項**頁面。 **建立 DNS 委派**選項時，使用現有的 DNS 伺服器基礎結構中建立新的樹系根 DNS 區域。 此選項可讓您提供其他 DNS 管理認證已更新 DNS 區域的權限。  
  
如需有關您是否需要建立 DNS 委派的詳細資訊，請查看[了解區域委派](https://technet.microsoft.com/library/cc771640.aspx)。  
  
#### <a name="additional-options"></a>其他選項  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)  
  
**的其他選項**頁面顯示 NetBIOS 的網域名稱，可讓您撤銷它。 根據預設，NetBIOS 網域名稱符合上所提供的完整的網域名稱的最左邊標籤**部署組態**頁面。 例如，如果您提供 corp.contoso.com 的完整的網域名稱，預設 NetBIOS 網域名稱是 CORP.  
  
如果 15 字元名稱或較少並不會衝突另一個 NetBIOS 名稱，這是不變。 如果它能與其他 NetBIOS 名稱衝突，數字會附加的名稱。 如果超過 15 字元名稱，精靈將會提供唯一、 被截斷的建議。 不論，精靈先驗證名稱未在使用透過 WINS 查詢，NetBIOS 廣播。  
  
如需有關有效的網域名稱，查看知識庫文章[適用於電腦、 網域、 網站及 Ou 命名 Active Directory 規格](https://support.microsoft.com/kb/909264)。  
  
#### <a name="paths"></a>路徑  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)  
  
**路徑**頁面上，可讓您覆寫預設資料夾位置的 AD DS 資料庫中資料庫交易登，並 SYSVOL 分享。 預設位置都在之 %systemroot (亦即 C:\Windows)。  
  
#### <a name="review-options-and-view-script"></a>檢視選項]，然後檢視指令碼  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)  
  
**評論選項**頁面上可讓您驗證您的設定，並確保您開始安裝之前，先符合您的需求。 這不是一個機會停止使用伺服器管理員安裝。 這是只要之前繼續進行設定，請先確認您的設定選項  
  
**評論選項**在伺服器管理員頁面也提供選擇性**檢視指令碼**按鈕，以建立包含目前 ADDSDeployment 設定成單一的 Windows PowerShell 指令碼 Unicode 文字檔案。 這可讓您在伺服器管理員圖形介面作為 Windows PowerShell 部署 studio。 若要設定選項，匯出設定，然後取消精靈使用 Active Directory Domain Services 組態精靈。 此程序會建立進一步修改或直接使用有效且語法正確範例。 例如：  
  
```powershell 
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSForest `  
-CreateDNSDelegation `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainName "corp.contoso.com" `  
-DomainNetBIOSName "CORP" `  
-ForestMode "Win2012" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NoRebootOnCompletion:$false `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> 伺服器管理員通常會填入所有引升級後不會依賴預設值 （因為它們可能會改變之間未來版本 Windows 的 service pack） 的值。 有一個例外此**-safemodeadministratorpassword** （在故意的指令碼省略） 引數。 若要強制確認的提示，請執行 cmdlet 互動時省略值。  
  
#### <a name="prerequisites-check"></a>必要條件核取  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)  
  
**請必要條件**是 AD DS 網域設定中的新功能。 這個新階段驗證伺服器設定可以新 AD DS 樹系的支援。  
  
當您安裝新的樹系根網域，伺服器管理員 Active Directory Domain Services 組態精靈會叫用一系列模組測試。 這些測試提醒建議的修復選項。 您可以視需要執行測試。 無法繼續網域控制站程序，直到所有必要條件測試傳遞。  
  
**請必要條件**也會呈現相關資訊，例如安全性變更會影響較舊的作業系統。  
  
如需有關的特定的必要條件檢查的詳細資訊，請查看[必要條件檢查](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
#### <a name="installation"></a>安裝  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)  
  
當**安裝**頁面會顯示，網域控制站設定開始和無法終止或取消。 詳細的作業會顯示在此頁面上，而且寫入登：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
> [!NOTE]  
> 您可以從同一部主機伺服器管理員中同時執行多個角色安裝和 AD DS 設定精靈。  
  
#### <a name="results"></a>結果  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
**結果**頁面會顯示成功或失敗的升級與管理的任何重要資訊。 網域控制站將會自動重新開機之後 10 秒。  
  
## <a name="BKMK_PSForest"></a>部署 Windows PowerShell 中的樹系  
本章節如何安裝森林根網域核心 Windows Server 2012 的電腦上使用 Windows PowerShell 中的第一個網域控制站。  
  
### <a name="windows-powershell-ad-ds-role-installation-process"></a>Windows PowerShell AD DS 角色安裝程序  
執行的幾個簡單 ServerManager 部署 cmdlet 到您的部署程序，您進一步了解的 AD DS 簡化管理。  
  
下圖顯示 Active Directory Domain Services 角色安裝程序，開頭為您執行的**PowerShell.exe**和結束網域控制站在升級之前的權限。  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)  
  
|||  
|-|-|  
|ServerManager Cmdlet|引數 (**粗體**所需的引數。 *斜體*引數可以使用 Windows PowerShell 或 AD DS 設定精靈指定。)|  
|安裝-WindowsFeature 日新增-WindowsFeature|***名稱***<br /><br />*-重新開機*<br /><br />*-IncludeAllSubFeature*<br /><br />*-IncludeManagementTools*<br /><br />來源<br /><br />*-電腦名稱*<br /><br />認證<br /><br />-LogPath<br /><br />*-Vhd*<br /><br />*-ConfigurationFilePath*|  
  
> [!NOTE]  
> 一些不需要，引數**-IncludeManagementTools**我們建議安裝 AD DS 角色二進位檔  
  
ServerManager 模組的 Windows PowerShell 公開角色安裝、 狀態，並移除的部分新 DISM 模組。 這個層簡化最工作並減少強大 （但時濫用危險） 直接使用量需要 DISM 模組。  
  
使用**Get 命令**將別名和中 ServerManager cmdlet 匯出。  
  
```powershell  
Get-Command -module ServerManager  
```  
  
例如：  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)  
  
若要新增的 Active Directory Domain Services 角色，只要執行**安裝-WindowsFeature**與引數 AD DS 角色名稱。 伺服器管理員，例如所有所需的服務隱含到 AD DS 角色自動安裝更新。  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services  
```  
  
如果您也可以安裝-AD DS 管理工具，這會建議-然後提供**-IncludeManagementTools**引數：  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools  
```  
  
例如：  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)  
  
請列出所有的功能與他們安裝狀態的角色，使用**取得-WindowsFeature**不引數。 指定**電腦名稱的**安裝狀態從遠端伺服器引數。  
  
```powershell  
Get-WindowsFeature  
```  
  
因為**取得-WindowsFeature**不需要篩選機制，您必須使用**Where-object**以管線尋找特定的功能。 管線之間傳送資料的多個 cmdlet 所使用的通道，而且 where-object 做為篩選。 建**$_**變數做為通過該光碟可能包含任何屬性管線目前物件。  
  
```powershell  
Get-WindowsFeature | where-object <options>  
```  
  
例如，尋找所有功能包含 「 作用中 Dir 」 中的**顯示名稱**屬性，使用：  
  
```powershell  
Get-WindowsFeature | where displayname -like "*active dir*"  
```  
  
進一步的範例如下所示：  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)  
  
如需更多的 Windows PowerShell 作業管線與 Where-object 的詳細資訊，請查看[傳送及 Windows PowerShell 中的管線](https://technet.microsoft.com/library/ee176927.aspx)。  
  
請注意，Windows PowerShell 3.0 大幅簡化的命令列需要這項操作管線引數。 Windows PowerShell 2.0 您必須：  
  
```powershell  
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}  
```  
  
您可以使用 Windows PowerShell 管線，建立可讀取的結果。 例如：  
  
```powershell  
Install-WindowsFeature | Format-List  
Install-WindowsFeature | select-object | Format-List  
  
```  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)  
  
請注意如何使用**選取物件**cmdlet 的**-expandproperty**引數傳回有趣的資料：  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)  
  
> [!NOTE]  
> **選擇物件-expandproperty**引數速度變慢整體安裝效能稍微。  
  
### <a name="BKMK_PS"></a>使用 Windows PowerShell 建立 AD DS 森林根網域  
若要安裝新的 Active Directory 森林使用 ADDSDeployment 模組，使用下列 cmdlet:  
  
```powershell  
Install-addsforest  
```  
  
**安裝-AddsForest** cmdlet 僅有兩個階段 （必要條件檢查並安裝）。 有兩個下方的數據會顯示安裝階段的最低的必要引數**的網域名稱**。  
  
|||  
|-|-|  
|ADDSDeployment Cmdlet|引數 (**粗體**所需的引數。 *斜體*引數可以使用 Windows PowerShell 或 AD DS 設定精靈指定。)|  
|安裝-Addsforest|-確認<br /><br />*-CreateDNSDelegation*<br /><br />*-DatabasePath*<br /><br />*-DomainMode*<br /><br />***-網域名稱***<br /><br />***-DomainNetBIOSName***<br /><br />*-DNSDelegationCredential*<br /><br />*-ForestMode*<br /><br />-推動<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-NoDnsOnNetwork<br /><br />-NoRebootOnCompletion<br /><br />*-SafeModeAdministratorPassword*<br /><br />-SkipAutoConfigureDNS<br /><br />-SkipPreChecks<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> **-DomainNetBIOSName**如果您想要變更自動根據 DNS 網域名稱前置詞的 15 字元名稱或名稱超過 15 字元，則需要引數。  
  
伺服器管理員相當於**部署組態**ADDSDeployment cmdlet 和引數：  
  
```powershell  
Install-ADDSForest  
-DomainName <string>  
```  
  
相當於伺服器管理員網域控制站選項 ADDSDeployment cmdlet 引數︰  
  
```powershell  
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-InstallDNS <{$false | $true}>  
-SafeModeAdministratorPassword <secure string>  
  
```  
  
**安裝-ADDSForest**如果您不指定引數請遵循相同的預設值為伺服器管理員。  
  
**SafeModeAdministratorPassword**引數的作業會特殊：  
  
-   如果*未指定*引數，cmdlet 會提示您輸入並確認遮罩的密碼。 執行 cmdlet 互動時，這是慣用的使用方式。  
  
    例如，建立新的樹系名 corp.contoso.com，並提示您輸入並確認密碼遮罩：  
  
    ```powershell  
    Install-ADDSForest "DomainName corp.contoso.com  
    ```  
  
-   如果指定*的值，*，值必須安全字串。 執行 cmdlet 互動時，這是不慣用的使用方式。  
  
例如，您可以手動提示密碼使用**朗讀主機**cmdlet 提示安全字串的使用者：  
  
```powershell  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> 在前一個選項不會確認密碼、 小心謹慎： 看不到密碼。  
  
您也可以提供安全字串為轉換明文變數，雖然這是非常不建議使用。  
  
```powershell  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
最後，您可能會將模糊的密碼儲存在檔案，並再重複使用之後，清除文字並不會顯示密碼。 例如：  
  
```powershell  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> 不建議提供或儲存清除或模糊文字密碼。 任何人指令碼執行這個命令或在您身邊尋找知道網域控制站 DSRM 的密碼。 任何人的存取權檔案無法反向模糊的密碼。 有了這個認知，他們可以登入以 DSRM 開始 DC 及最後模擬網域控制站本身他們的權限提高 Active Directory 森林中的最高層級。 步驟使用另一組**System.Security.Cryptography**來將檔案加密資料建議但是超出範圍。 最好的做法是完全避免儲存的密碼。  
  
ADDSDeployment cmdlet 提供略過 DNS client 設定、 轉送程式，以及根提示自動設定的其他選項。 您不能略過此組態選項時使用伺服器管理員。 此引數重要只有當您在安裝前設定的網域控制站伺服器的 DNS 伺服器角色：  
  
```powershell  
-SkipAutoConfigureDNS  
```  
  
**DomainNetBIOSName**也是特殊操作：  
  
-   如果**DomainNetBIOSName**未使用 NetBIOS 的網域名稱和單一標籤前置詞網域中的名稱指定引數**網域名稱、** 15 字元或較少，然後升級繼續使用自動的名稱。  
  
-   如果**DomainNetBIOSName**未使用 NetBIOS 的網域名稱和單一標籤前置詞網域中的名稱指定引數**網域名稱、** 16 字元或更多，然後升級失敗。  
  
-   如果**DomainNetBIOSName**指定引數 NetBIOS 網域名稱的 15 字元或較少，然後升級繼續指定名稱。  
  
-   如果**DomainNetBIOSName**指定引數字元 16 NetBIOS 網域名稱或更多，然後升級失敗。  
  
相當於伺服器管理員其他選項 ADDSDeployment cmdlet 引數是：  
  
```powershell  
-domainnetbiosname <string>  
```  
  
伺服器管理員相當於**路徑**ADDSDeployment cmdlet 引數：  
  
```powershell  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
  
```  
  
使用選擇性**Whatif**以引數**安裝-ADDSForest** cmdlet 檢視設定的資訊。 這可讓您查看明確和隱含 cmdlet 的引數的值。  
  
例如：  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)  
  
您無法略過**必要條件檢查**時使用伺服器管理員中，但您可以跳過此程序使用 [使用下列引數 AD DS 部署 cmdlet 時：  
  
```powershell  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft 會阻礙重覆它會導致部分網域控制站升級或損壞 AD DS 森林略過必要條件檢查。  
  
請注意，就像伺服器管理員中，**安裝-ADDSForest** ，升級將會自動重新開機伺服器提醒您。  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)  
  
![安裝新的樹系](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)  
  
若要自動接受重新開機命令提示字元中，使用**-強制**或**-確認： $false**的任何 ADDSDeployment Windows PowerShell cmdlet 引數。 若要防止伺服器促銷結尾自動重新開機，使用**-norebootoncompletion**引數。  
  
> [!WARNING]  
> 覆寫在重新開機，建議。 網域控制站必須重新開機才能正確運作。  
  
## <a name="see-also"></a>也了  
[Active Directory Domain Services （TechNet 入口網站）](https://technet.microsoft.com/library/cc770946(WS.10).aspx)  
[Windows Server 2008 R2 的 active Directory Domain Services](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
[Windows Server 2008 的 active Directory Domain Services](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
[Windows Server Technical 參考資料 (Windows Server 2003)](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
[Active Directory 系統管理員中心： 快速入門 (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd560651(WS.10).aspx)  
[Active Directory 管理，使用 Windows PowerShell (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd378937(WS.10).aspx)  
[詢問 Directory 服務小組 （官方的 Microsoft 廣告技術支援部落格）](http://blogs.technet.com/b/askds)  
  

