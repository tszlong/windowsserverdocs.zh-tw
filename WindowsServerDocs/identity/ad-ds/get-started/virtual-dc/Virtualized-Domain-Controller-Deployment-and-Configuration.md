---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: "模擬的網域控制站部署和設定"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dcb377cef003458bcdf2e4b3564167cee4310020
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>模擬的網域控制站部署和設定

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題涵蓋：  
  
-   [安裝考量](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    這包括平台需求和其他重要限制。  
  
-   [擬化檔案網域控制站複製](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    這解釋詳細整個模擬的網域控制站複製程序。  
  
-   [模擬的網域控制站安全還原](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    這解釋詳細模擬的網域控制站安全還原時進行驗證。  
  
## <a name="BKMK_InstallConsiderations"></a>安裝考量  
未特殊角色或模擬的網域控制站; 安裝的功能所有網域控制站自動都包含複製且安全還原功能。 您無法移除或停用這些功能。  
  
使用 Windows Server 2012 網域控制站需要的 Windows Server 2012 AD DS 架構 56 或更高版本和樹系功能等級等於 Windows Server 2003 原生或更高版本。  
  
這兩個寫入和唯讀的網域控制站支援模擬俠的各個層面全球目錄和 FSMO 角色一樣。  
  
> [!IMPORTANT]  
> 複製開始時，必須 online PDC 模擬器 FSMO 角色擁有者。  
  
### <a name="BKMK_PlatformReqs"></a>平台需求  
模擬網域控制站複製需要：  
  
-   在 Windows Server 2012 網域控制站裝載 PDC 模擬器 FSMO 角色  
  
-   使用複製的作業時肯定  
  
複製和安全還原需要：  
  
-   Windows Server 2012 擬化檔案來賓  
  
-   模擬主機平台支援 VM 新一代 ID (VMGID)  
  
檢視模擬你下的表，並是否支援擬化檔案網域控制站及 VM 新一代 id。  
  
|||  
|-|-|  
|**模擬 Product**|**網域控制站和 VMGID 擬化檔案支援**|  
|**Microsoft Windows Server 2012 伺服器 HYPER-V 功能**|[是]|  
|**Microsoft Windows Server 2012 HYPER-V Server**|[是]|  
|**Microsoft Windows 8 HYPER-V Client 的功能**|[是]|  
|**Windows Server 2008 R2 和 Windows Server 2008**|否]|  
|**非 Microsoft 模擬方案**|請連絡製造商|  
  
即使 Microsoft 支援 Windows 7 Virtual PC、 Virtual PC 2007、 Virtual PC 2004 年和 Virtual Server 2005，他們無法執行 64 位元來賓，也不他們支援 VM-GenerationID。  
  
第三方模擬你協助和模擬的網域控制站其支援態勢，請直接連絡該廠商。  
  
如需詳細資訊，檢視支援原則[Microsoft 軟體非 Microsoft 硬體的軟體模擬執行](https://support.microsoft.com/kb/897615)。  
  
### <a name="critical-caveats"></a>重要的注意事項  
模擬的網域控制站執行*未*支援安全還原的動作：  
  
-   VHD 和 VHDX 檔案手動覆寫現有的 VHD 檔案  
  
-   VHD 和 VHDX 檔案還原使用檔案備份或完整磁碟備份軟體  
  
> [!NOTE]  
> VHDX 檔案是以 Windows Server 2012 HYPER-V 新。  
  
這兩個這些作業涵蓋在 VM-GenerationID 語意，因此不會變更 VM 新一代編號。 還原的網域使用這些方法控制器可能會造成 USN 回復，並隔離的網域控制站或介紹延遲物件和需要的樹系的寬形清除作業。  
  
> [!WARNING]  
> 模擬的網域控制站安全還原並不備份系統狀態並 AD DS 資源回收筒]。  
>   
> 還原快照之後, 將會永久遺失的開發進程的快照之後來自網域控制站之前取消複寫變更 delta。 安全還原實作以防止誤網域控制站隔離自動化未經授權還原*只*。  
  
如需有關 USN 泡泡延遲物件，請[失敗，錯誤 8606 疑難排解 Active Directory 作業: 「 不足屬性已給予建立物件]](https://support.microsoft.com/kb/2028495)。  
  
## <a name="BKMK_VDCCloning"></a>擬化檔案網域控制站複製  
有許多階段與複製模擬的網域控制站，不論使用的圖形工具或 Windows PowerShell 中的步驟。 高階，三個階段︰  
  
**準備環境**  
  
-   步驟 1： 驗證 hypervisor 支援 VM 新一代 ID 和因此，複製  
  
-   步驟 2： 驗證期間複製的網域控制站執行 Windows Server 2012 和是 online，且可以複製的網域控制站裝載 PDC 模擬器角色。  
  
**準備來源網域控制站**  
  
-   步驟 3： 授權的來源網域控制站複製  
  
-   步驟 4： 移除相容服務或程式，或將它們新增到 CustomDCCloneAllowList.xml 檔案。  
  
-   步驟 5： 建立 DCCloneConfig.xml  
  
-   步驟 6： 拍攝來源網域控制站 offline  
  
**建立複製的網域控制站**  
  
-   步驟 7： 複製或匯出 VM 的來源，並加入 XML 如果尚未複製  
  
-   步驟 8： 建立新的一樣從複本  
  
-   [開始] 的新一樣開始複製到執行 「 步驟 9:  
  
當使用圖形工具，例如 HYPER-V Management Console 或命令列工具，例如 Windows PowerShell，步驟會看到一次介面有操作無程序不同。 本主題提供適用於您探索的複製程序; 端點-自動化的 Windows PowerShell 範例他們並不需要任何步驟。 還有圖形管理工具，則模擬的網域控制站包含 Windows Server 2012 中。  
  
有幾個點程序，您有選擇如何建立複製的電腦，以及您如何將 xml 檔案。下列步驟進行所述的下列詳細資料。 此程序會否則不變。  
  
下圖顯示模擬的網域控制站複製程序，已經有網域。  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>步驟 1-驗證 Hypervisor  
確定支援 hypervisor 執行來源網域控制站查看廠商文件。 模擬的網域控制站 hypervisor 無關並不需要 HYPER-V。  
  
如果 Microsoft HYPER-V hypervisor，確定它已在 Windows Server 2012 上執行。 您可以驗證這使用管理的裝置  
  
開放**Devmgmt.msc** ，然後檢查**系統裝置**適用於已安裝 Microsoft HYPER-V 裝置和驅動程式。 所需的模擬的網域控制站裝置特定的系統**Microsoft HYPER-V 代計數器**(驅動程式︰ vmgencounter.sys)。  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>步驟 2-驗證 PDCE FSMO 角色  
您嘗試複製 DC 之前，您將必須驗證網域控制站裝載主要網域控制站模擬器 FSMO 執行 Windows Server 2012。 肯定 (PDCE) 是需要幾個原因：  
  
1.  PDCE 建立特殊**的網域控制站複製**群組和設定的網域允許網域控制站複製本身根本身的權限。  
  
2.  複製網域控制站連絡人使用 DRSUAPI RPC 通訊協定，以建立電腦物件的複製俠直接 PDCE。  
  
    > [!NOTE]  
    > Windows Server 2012 延伸現有 Directory 複寫服務 (DRS) 遠端通訊協定 (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**) 包含新的 RPC 方法**IDL_DRSAddCloneDC** (Opnum **28**)。 **IDL_DRSAddCloneDC**方法建立新的網域控制站物件複製從現有的網域控制站物件的屬性。  
    >   
    > 網域控制站的狀態的電腦，伺服器、 NTDS 設定、 FRS、 DFSR 和連接物件針對每個網域控制站維護所組成。 複製物件，當這個 RPC 方法會取代原始的網域控制站的所有參考使用新的網域控制站的對應的物件。 播報來電者必須控制項存取權限 DS-複製-網域控制站在網域命名操作。  
    >   
    > 這個新的方法使用一律會需要 PDC 模擬器網域控制站直接存取從播報來電者。  
    >   
    > 這個 RPC 方法的新功能，因為您的網路分析的軟體需要更新以包含欄位中現有 UUID E3514235-4B06-11 D 1-AB04-00C04FC2DCD2 新 Opnum 28 的分析。 否則，您不能剖析此資料傳輸。  
    >   
    > 如需詳細資訊，請查看[4.1.29 IDL_DRSAddCloneDC (Opnum 28)](https://msdn.microsoft.com/en-us/library/hh554213(v=prot.13).aspx)。  
  
***這也表示使用非完全路由的網路時模擬的網域控制站複製需要 PDCE 存取網路區段***。 接受之後，只要您有仔細地更新 AD DS 邏輯網站資訊複製-一樣實體網域控制站-移動到不同的網路的複製的網域控制站是。  
  
> [!IMPORTANT]  
> 複製包含只有單一網域控制站網域，當您必須確定來源 DC 恢復先複製複本。 Production 網域一律應至少兩個網域控制站。  
  
#### <a name="active-directory-users-and-computers-method"></a>Active Directory 使用者和電腦方法  
  
1.  使用 [Dsa.msc 嵌入式管理單元，網域上按一下滑鼠右鍵，然後按一下**操作主機**。 請注意名 PDC 索引標籤上的網域控制站，並關閉對話方塊。  
  
2.  以滑鼠右鍵按一下該俠的電腦，然後按一下**屬性**，然後驗證的作業系統資訊。  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
您可結合下列 Active Directory Windows PowerShell 模組 cmdlet 返回肯定版本：  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
如果未提供網域，這些 cmdlet 假設網域的電腦執行的位置。  
  
下列命令傳回 PDCE 和的作業系統資訊：  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
此以下的範例指定的網域名稱及篩選之前的 Windows PowerShell 管線傳回的屬性：  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>步驟 3-授權俠來源  
來源網域控制站必須控制存取權限 （汽車）**允許建立自己的複本 DC**上 NC 標頭的網域。 根據預設，已知群組**複製網域控制站**有此權限，以及不包含任何成員。 PDCE 該 FSMO 角色傳輸網域控制站 Windows Server 2012 時，會建立此群組。  
  
#### <a name="active-directory-administrative-center-method"></a>Active Directory 系統管理員中心方法  
  
1.  開始 Dsac.exe 並瀏覽至來源 DC，然後打開其詳細資料頁面。  
  
2.  在**成員的**區段，我們新增**複製網域控制站**的群組。  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
您可結合下列 Active Directory Windows PowerShell 模組 cmdlet**取得-adcomputer**和**新增 adgroupmember**加入網域控制站**複製網域控制站**群組：  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
例如，這會將伺服器 DC1 加入該群組，而不需要的群組成員分辨的名稱指定：  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>重建預設的權限  
如果您的網域標頭移除此權限，請複製失敗。 您可以重新使用 Windows PowerShell 的 Active Directory 管理中心權限。  
  
##### <a name="active-directory-administrative-center-method"></a>Active Directory 系統管理員中心方法  
  
1.  開放**Active Directory 管理中心**，以滑鼠右鍵按一下網域標頭，按一下 [**屬性**，按一下**擴充功能**索引標籤上，按一下 [**安全性**，然後按一下 [**進階**。 按一下**這個物件只**。  
  
2.  按一下**新增**的**輸入物件名稱來選取 [**，輸入群組名稱**複製網域控制站。**  
  
3.  權限] 下，按一下 [**允許建立自己的複本 DC**，然後按一下 [ **[確定]**。  
  
> [!NOTE]  
> 您也可以移除預設的權限，以及新增個人網域控制站。 這是可能造成不少問題，不過，新的系統管理員是不知道這個自訂的位置。 變更預設不提高安全性，而且不建議使用。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
在已提升權限管理員 Windows PowerShell 主機命令提示字元使用下列命令。 這些命令偵測到的網域名稱，新增入預設的權限：  
  
```  
import-module activedirectory  
cd ad:  
$domainNC = get-addomain  
$dcgroup = get-adgroup "Cloneable Domain Controllers"  
$sid1 = (get-adgroup $dcgroup).sid  
$acl = get-acl $domainNC  
$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e  
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid  
$acl.AddAccessRule($ace1)  
set-acl -aclobject $acl $domainNC  
cd c:  
```  
  
或者，執行範例[FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)在 Windows PowerShell 主控台中，以提升權限網域控制站受影響的網域中的系統管理員身分在主機開始的位置。 它會自動設定的權限。 範例位於這個模組本文附件。  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>步驟 4-移除不相容應用程式或服務 （如果不使用 CustomDCCloneAllowList.xml）  
任何程式或服務所取得-ADDCCloningExcludedApplicationList-先前傳回*未加入 CustomDCCloneAllowList.xml 和*-之前複製必須移除。 解除安裝的應用程式或服務是建議的方法。  
  
> [!WARNING]  
> 任何不相容的程式或服務無法解除安裝或新增至 CustomDCCloneAllowList.xml 防止複製。  
  
使用中找不到任何獨立管理服務帳號 (MSAs) 網域取得-AdComputerServiceAccount cmdlet，如果這台電腦使用這其中任一個。 如果已安裝任何 MSA，使用解除安裝-ADServiceAccount cmdlet 移除帳號本機安裝的服務。 一旦您完成在執行 「 步驟 6 拍攝來源網域控制站 offline，您可以重新新增恢復伺服器時使用 ADServiceAccount 安裝 MSA。 如需詳細資訊，請查看[ADServiceAccount 解除安裝的](https://technet.microsoft.com/en-us/library/hh852310)。  
  
> [!IMPORTANT]  
> 第一次在 Windows Server 2008 R2 推出的-獨立 MSAs 所取代群組 MSAs 與 Windows Server 2012 中。 群組 MSAs 支援複製。  
  
### <a name="step-5---create-dccloneconfigxml"></a>步驟 5-建立 DCCloneConfig.xml  
需要複製網域控制站的 DcCloneConfig.xml 檔案。 內容可讓您指定獨特的詳細資訊，例如新的電腦名稱與 IP 位址。  
  
除非您安裝的應用程式或可能不相容的 Windows 服務來源網域控制站 CustomDCCloneAllowList.xml 檔案是選擇性的。 將檔案需要精確命名、 格式和位置。否則，複製失敗。  
  
基於這個原因，您一定要使用 Windows PowerShell cmdlet 建立 XML 檔案，並將它們放在正確的位置。  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>使用新 ADDCCloneConfigFile 產生  
Active Directory Windows PowerShell 模組包含 Windows Server 2012 中的新 cmdlet:  
  
```  
New-ADDCCloneConfigFile  
```  
  
您建議的來源網域控制站您想複製到執行 cmdlet。 Cmdlet 支援多個引數，並使用時，一律測試的環境，它會執行除非您指定與電腦-離線引數。  
  
||||  
|-|-|-|  
|**ActiveDirectory**<br /><br />**Cmdlet**|**引數**|**解釋**|  
|**新 ADDCCloneConfigFile**|*<no argument specified>*|在 DSA 運作 Directory 中建立空白 DcCloneConfig.xml 檔案 (預設值︰ %systemroot%\ntds)|  
||-CloneComputerName|指定複製俠電腦名稱。 資料類型的字串。|  
||路徑|指定要建立 DcCloneConfig.xml 的資料夾。 如果您不指定，寫入 DSA 運作 Directory (預設值︰ %systemroot%\ntds)。 資料類型的字串。|  
||-站台名稱|指定加入複製的電腦 account 建立期間 AD 邏輯網站名稱。 資料類型的字串。|  
||-IPv4Address|指定複製電腦靜態 IPv4 位址。 資料類型的字串。|  
||-IPv4SubnetMask|指定靜態 IPv4 子網路遮罩複製的電腦。 資料類型的字串。|  
||-IPv4DefaultGateway|指定複製電腦的靜態 IPv4 預設閘道位址。 資料類型的字串。|  
||-IPv4DNSResolver|指定 IPv4 DNS 項目複製電腦的靜態以逗號分隔的清單。 陣列資料類型。 最四個項目提供。|  
||-PreferredWINSServer|指定主要 WINS 伺服器靜態 IPv4 位址。 資料類型的字串。|  
||-AlternateWINSServer|指定次要 WINS 伺服器靜態 IPv4 位址。 資料類型的字串。|  
||-IPv6DNSResolver|指定 IPv6 DNS 項目複製電腦的靜態以逗號分隔的清單。 還有不模擬的網域控制站複製設定 Ipv6 靜態資訊的方式。 陣列資料類型。|  
||-Offline|不會執行驗證測試，並會覆寫任何的現有 dccloneconfig.xml。 不具有參數。 如需詳細資訊，請查看[執行新-ADDCCloneConfigFile 離線模式在](../../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode)。|  
||*靜態*|如果指定靜態 IP 引數 IPv4SubnetMask、 IPv4SubnetMask 或 IPv4DefaultGateway，需要。 不具有參數。|  
  
測試執行時 online 模式中執行：  
  
-   肯定是 Windows Server 2012 或更新版本  
  
-   來源網域控制站是複製網域控制站群組成員  
  
-   來源網域控制站不包含任何排除的應用程式或服務  
  
-   來源網域控制站尚未包含 DcCloneConfig.xml 指定的路徑。  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>步驟 6-拍攝來源網域控制站 Offline  
您無法將的執行來源俠; 複製它必須關機正常。 請勿複製網域控制站停止 graceless 電源中斷。  
  
#### <a name="graphical-method"></a>圖形方法  
使用 DC 執行，在 [關機] 按鈕或 [HYPER-V 管理員關機] 按鈕。  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
您可以關閉一樣，使用下列 cmdlet 其中一項：  
  
```  
Stop-computer  
Stop-vm  
```  
  
停止電腦是 cmdlet 支援關機模擬，無論電腦及類似傳統 Shutdown.exe 公用程式。 停止-vm 新 cmdlet 在 Windows Server 2012 HYPER-V Windows PowerShell 模組，並相當於 HYPER-V 管理員電源選項。 第二個適用於 lab 位置的網域控制站通常運作模擬私人網路的環境。  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>步驟 7-複製磁碟  
管理選擇需要在複製階段：  
  
-   複製磁碟手動，而不需要 HYPER-V  
  
-   使用 HYPER-V VM 匯出  
  
-   匯出合併的磁碟，使用 HYPER-V  
  
必須複製所有一樣的磁碟，而不只系統磁碟機。 如果來源網域控制站使用差異磁碟且想要將您複製的網域控制站移到另一個 HYPER-V 主機，您必須匯出。  
  
如果來源網域控制站僅有手動複製磁碟建議*一個*磁碟機。 / 匯出建議 vm 的*多個*磁碟機或其他複雜的模擬的硬體的自訂項目像多個 Nic。  
  
手動複製檔案時，如果 delete 之前複製任何快照。 若要匯出 VM、 delete 快照之前匯出或加以移除新 VM 匯入後。  
  
> [!WARNING]  
> 快照的不同磁碟，可以網域控制站回到先前的狀態。 如果您要複製的網域控制站並還原預先複製快照，最後會森林中的重複的網域控制站的。 不還有任何值先前快照中新複製的網域控制站。  
  
#### <a name="manually-copying-disks"></a>手動複製磁碟  
  
##### <a name="hyper-v-manager-method"></a>HYPER-V 管理員方法  
使用 HYPER-V 管理員嵌入式管理單元，以判斷來源網域控制站相關聯的磁碟。 使用檢查選項來驗證網域控制站使用差異磁碟 （這需要複製父系磁碟也）  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
Delete 快照，並選取 VM delete 快照樹狀。  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
您可以再手動複製使用 Windows 檔案總管、 Xcopy.exe 或 Robocopy.exe VHD 或 VHDX 檔案。 不特殊的步驟會需要。 最好變更檔名，即使移動到另一個資料夾。  
  
> [!NOTE]  
> 如果主機區域網路上的電腦之間複製 (1-Gbit 或更高)、 **Xcopy.exe /J**選項 VHD 日 VHDX 將檔案複製任何其他工具，但這會容量較大的頻寬降低比變快。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要判斷磁碟使用 Windows PowerShell，使用 HYPER-V 模組：  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
例如，您可以從 VM 名傳回所有 IDE 硬碟**DC2**的下列範例：  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
如果磁碟路徑 AVHD 或 AVHDX 的檔案，則開發進程的快照。 若要 delete 相關聯的磁碟和合併真實 VHD 或 VHDX 快照，使用 cmdlet:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
例如，以 delete 所有快照從 VM 命名為 DC2-SOURCECLONE:  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
使用 Windows PowerShell 將檔案複製，使用下列 cmdlet:  
  
```  
Copy-Item  
```  
  
結合 VM cmdlet 來幫助自動化管線與。 管線是之間傳送資料的多個 cmdlet 所使用的通道。 例如，複製離線來源網域控制站的磁碟機命名為 DC2-SOURCECLONE 到新磁碟稱為 c:\temp\copy.vhd 而不需要知道確切路徑其系統磁碟機：  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> 您無法使用過渡磁碟複製、 為它們不會使用 virtual 磁碟檔案而實際硬碟。  
  
> [!NOTE]  
> 如需有關更多的 Windows PowerShell 作業管線的詳細資訊，請查看[傳送及 Windows PowerShell 中的管線](https://technet.microsoft.com/en-us/library/ee176927.aspx)。  
  
#### <a name="exporting-the-vm"></a>匯出 VM  
複製磁碟的替代方案，您可以匯出整個的 HYPER-V VM 做為備份。 自動匯出建立資料夾 vm 命名，並包含所有的磁碟和設定的資訊。  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>HYPER-V 管理員方法  
若要匯出的管理員 HYPER-V VM:  
  
1.  來源網域控制站上按一下滑鼠右鍵，然後按一下**匯出**。  
  
2.  做為匯出容器選取現有的資料夾。  
  
3.  等待停止顯示 \ [狀態] 欄**匯出**。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要匯出使用 HYPER-V Windows PowerShell 模組 VM 中，使用 cmdlet:  
  
```  
Export-vm  
```  
  
例如，匯出 VM 命名為 DC2-SOURCECLONE 名 C:\VM 的資料夾：  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> 新的 Windows Server 2012 HYPER-V 支援匯出與匯入的此訓練範圍的功能。 檢視 TechNet 如需詳細資訊。  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>使用 HYPER-V 的匯出合併的磁碟]  
使用 HYPER-V 中的磁碟跨欄轉換選項是最後的選項。 這些可讓您將一份現有的磁碟結構-即使是在包含快照 AVHD 日 AVHDX 檔案時-成單一的新磁碟。 手動磁碟複製案例中，例如此主要是僅使用單一磁碟機，例如 C:\\ 簡單虛擬電腦。 其單獨利用是的然而手動複製，不需要您第一次 delete 快照。 這項操作是一定低於只要刪除快照和複製磁碟。  
  
##### <a name="hyper-v-manager-method"></a>HYPER-V 管理員方法  
若要建立磁碟合併使用 HYPER-V 管理員：  
  
1.  按一下**編輯磁碟**。  
  
2.  針對最低的子女磁碟瀏覽。 例如，如果您使用差異磁碟，子女磁碟已最低的子女。 如果一樣快照 （或多個列出），選取目前開發進程的快照是最低子女磁碟。  
  
3.  選取 [**合併**選項來建立退出整個的父系子女結構單一磁碟。  
  
4.  選取 [新增 virtual 硬碟與提供的路徑。 這一個新可移植單位風險還原之前快照的不是插入協調現有 VHD 日 VHDX 檔案。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要建立合併的磁碟的一組複雜的家長使用 HYPER-V Windows PowerShell 模組，使用 cmdlet:  
  
```  
Convert-vm  
```  
  
例如，匯出整個一連串 VM 的磁碟快照 （不包括任何差異磁碟這次） 與到新的單一磁碟父磁碟命名為 DC4 複製。VHDX:  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="BKMK_Offline"></a>新增 XML 離線系統磁碟  
如果您未複製 Dccloneconfig.xml DC 的執行來源，您必須將更新的 dccloneconfig.xml 檔案複製到離線複製/匯出系統磁碟現在。 根據稍早偵測到取得-ADDCCloningExcludedApplicationList 安裝的應用程式，您也可能需要 CustomDCCloneAllowList.xml 檔案複製到磁碟。  
  
在下列位置可以包含 DcCloneConfig.xml 檔案：  
  
1.  使用 Directory DSA  
  
2.  %windir%\NTDS  
  
3.  讀取/寫入卸除式媒體，在磁碟機代號，在磁碟機的根的訂單  
  
這些路徑不進行設定。 複製開始之後，複製檢查該特定順序和第一次 DcCloneConfig.xml 使用這些位置檔案找到，不論的其他資料夾。  
  
在下列位置可以包含 CustomDCCloneAllowList.xml 檔案：  
  
1.  HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters  
  
    AllowListFolder (*REG_SZ*)  
  
2.  使用 Directory DSA  
  
3.  %windir%\NTDS  
  
4.  讀取/寫入卸除式媒體，在磁碟機代號，在磁碟機的根的訂單  
  
您可以執行新-ADDCCloneConfigFile 使用**-離線**引數 （也稱為離線模式），以建立 DcCloneConfig.xml 檔案，並將它放在正確的位置。 下列範例顯示如何執行離線模式中的新-ADDCCloneConfigFile。  
  
若要建立複製網域控制站名 CloneDC1 離線模式，靜態 IPv4 位址，稱為 「 REDMOND 「 網站中輸入：  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
若要建立複製網域控制站名 Clone2 靜態 IPv4 與靜態 IPv6 設定中，輸入離線模式：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
若要建立複製網域控制站離線模式靜態 IPv4 與動態 IPv6 設定中指定 DNS 解析設定多個 DNS 伺服器，鍵入：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
若要建立複製網域控制站名 Clone1 動態 IPv4 與靜態 IPv6 設定中，輸入離線模式：  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
若要建立複製網域控制站在動態 IPv4 與動態 IPv6 設定離線模式，請輸入：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Windows 檔案總管的方法  
Windows Server 2012 現在提供圖形裝載 VHD 和 VHDX 檔案的選項。 這需要安裝在 Windows Server 2012 上桌面體驗功能。  
  
1.  按一下含有來源俠的系統磁碟機或 DSA 運作 Directory 位置的資料夾，新複製的 VHD 日 VHDX 檔案，然後按一下**雷**的**光碟影像工具**功能表。  
  
2.  現在裝載的磁碟機，XML 檔案複製到有效的位置。 您可能會資料夾的權限提示。  
  
3.  按一下 [裝載的磁碟機，然後按一下**退出**的**磁碟工具**功能表。  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVClickMountedDrive.png)  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDetailsMountedDrive.gif)  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVEjectMountedDrive.gif)  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
或者，您可以雷離線磁碟，然後使用 Windows PowerShell cmdlet 將 XML 檔案複製：  
  
```  
mount-vhd  
get-disk  
get-partition  
get-volume  
Add-PartitionAccessPath  
Copy-Item  
```  
  
這可讓您掌控程序。 例如，在磁碟機可以裝載的特定磁碟機代號，複製的檔案，以及解下磁碟機。  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
例如：  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
或者，您可以使用新的**DiskImage 山的**cmdlet 裝載 VHD （或 ISO） 的檔案。  
  
### <a name="step-8---create-the-new-virtual-machine"></a>步驟 8-建立新一樣  
最後的設定步驟開始複製程序之前已建立新 VM 使用複製的來源網域控制站的磁碟。 視磁碟階段複製中所做選擇，您有兩個選項：  
  
1.  新 VM 關聯複製磁碟  
  
2.  匯入匯出 VM  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>新 VM 關聯複製磁碟  
如果您要複製的系統磁碟以手動方式，您必須建立新的一樣使用複製的磁碟。 Hypervisor 時，自動設定 VM 新一代 ID 建立新 VM;任何設定變更，在 VM 或 HYPER-V 主機不需要。  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>HYPER-V 管理員方法  
  
1.  建立新的一樣。  
  
2.  指定 VM 名稱、 記憶體及網路。  
  
3.  在連接 Virtual 硬碟頁面上指定的複製的系統磁碟。  
  
4.  完成精靈，以建立 VM。  
  
如果有多個磁碟、 網路介面卡或其他的自訂項目，先網域控制站設定。 「 匯出匯入] 的方法複製磁碟複雜 Vm 的建議。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
您可以使用 HYPER-V Windows PowerShell 模組間，將 VM 建立 Windows Server 2012，使用下列 cmdlet 中：  
  
```  
New-VM  
```  
  
例如，以下 DC4-CLONEDFROMDC2 VM，使用建立 1 GB ram，開機從 c:\vm\dc4-systemdrive-clonedfromdc2.vhd 檔案，並使用 10.0 virtual 網路：  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>匯入 VM  
如果您先前匯出您 VM 之後，您要匯入該回做為備份。 這會使用匯出的 XML 重新建立使用所有先前的設定、 磁碟、 網路和記憶體設定的電腦。  
  
如果您想要從相同匯出 VM 建立其他複本，視需要進行 VM 匯出多個複本。 然後使用匯入的每個複本。  
  
> [!IMPORTANT]  
> 請務必使用**複製**選項，匯出會保留所有資訊來源。匯入的伺服器**移動**或**中的地方**如果相同 HYPER-V 主機伺服器上執行此動作，會導致資訊衝突。  
  
##### <a name="hyper-v-manager-method"></a>HYPER-V 管理員方法  
若要匯入使用 HYPER-V 管理員：  
  
1.  按一下**一樣匯入**  
  
2.  在**找出資料夾**頁面上，選取 [匯出的 VM 定義檔使用瀏覽按鈕  
  
3.  在**選擇一樣**頁面上，按一下 [來源電腦。  
  
4.  在**選擇匯入類型**頁面上，按一下 [**複製一樣 （建立新的唯一 ID）**，然後按一下 [**完成]**。  
  
5.  若要匯入上相同的 HYPER-V 主機; 重新命名匯入的 VM它將會有匯出的來源網域控制站相同的名稱。  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
請記住移除 HYPER-V 嵌入式管理單元使用任何匯入的快照：  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> 刪除任何匯入的快照時極為重要。如果套用，它們會回到複製的網域控制站的狀態先前-以及可能動態-DC，導致︰ 複寫失敗、 重複 IP 資訊，以及其他受到干擾。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
您可以使用 HYPER-V Windows PowerShell 模組間，將 VM 匯入 Windows Server 2012，使用下列 cmdlet 中：  
  
```  
Import-VM  
Rename-VM  
```  
  
例如 here 匯出使用其自動判斷的 XML 檔案，然後立刻重新命名為新 VM 名稱 DC5-CLONEDFROMDC2 VM DC2 複製匯入：  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
請記住，若要移除的任何匯入的快照，使用下列 cmdlet:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
例如：  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]  
> 確定，當匯入電腦，靜態的 MAC 位址已不指派網域控制站來源。 如果複製靜態 MAC 的來源電腦，將不正確會傳送或接收任何網路流量複製的電腦。 如果這種情形，設定新唯一靜態或動態的 MAC 位址。 您可以看到 VM 是否使用的命令靜態的 MAC 位址：  
>   
> **取得-VM-VMName**   
>  ***測試 vm 的*|取得-VMNetworkAdapter |fl \ ***  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>步驟 9-複製新一樣  
（選擇性） 複製您開始之前，請重新開機離線複製來源網域控制站。 請確定肯定 online，無論。  
  
若要開始複製，只需開始新一樣。 此程序會自動初始化並網域控制站自動重新開機之後複製完成。  
  
> [!IMPORTANT]  
> 不建議維持關閉一段時間為網域控制站和複製加入做為來源俠相同的網站，如果的初始站和間網站複寫拓撲可能需要較長的時間來源網域控制站離線是否組建。  
  
如果您可以使用 Windows PowerShell 來開始 VM，新的 HYPER-V 模組 cmdlet 是：  
  
```  
Start-VM  
```  
  
例如：  
  
![模擬的俠部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
電腦重新開機複製完成之後，一旦是網域控制站，您可以登入上通常確認正常運作。 如果有任何錯誤，開始 Directory 服務還原模式中的調查設定伺服器。  
  
## <a name="BKMK_VDCSafeRestore"></a>模擬防護功能  
之前模擬的網域控制站複製不同的是 Windows Server 2012 模擬防護功能會有任何設定步驟。 此功能運作介入，只要將您認識某些簡單的條件：  
  
-   Hypervisor 支援 VM 新一代 ID  
  
-   還有有效協力廠商網域控制站還原的網域控制站可以非系統授權複寫變更。  
  
### <a name="validate-the-hypervisor"></a>驗證 Hypervisor  
確定支援 hypervisor 執行來源網域控制站查看廠商文件。 模擬的網域控制站 hypervisor 無關並不需要 HYPER-V。  
  
檢視先前[平台需求](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs)區段已知 VM 新一代 ID 支援。  
  
如果您的移轉的來源 hypervisor Vm，以不同的目標 hypervisor，可能會模擬保護，或可能不會觸發根據 hypervisors 是否支援 VM 新一代 ID，如下表所述。  
  
|來源 hypervisor|目標 hypervisor|結果|  
|---------------------|---------------------|----------|  
|支援 VM 新一代 ID|不支援 VM 新一代 ID|不觸發防護功能 （如果有 DCCloneConfigFile.xml，DC 將開機進入 DSRM）|  
|不支援 VM 新一代 ID|支援 VM 新一代 ID|觸發防護功能|  
|支援 VM 新一代 ID|支援 VM 新一代 ID|不觸發 VM 定義未變更，這表示 VM 新一代 ID 保持不變，因為防護功能|  
  
### <a name="validate-the-replication-topology"></a>驗證複寫拓撲  
模擬保護起始未經授權的輸入的複寫的 delta 複寫 Active Directory，以及在同步未經授權的所有 SYSVOL 內容。 這樣可確保網域控制站的完整功能的開發進程的快照傳回和最後一致的環境中的其餘部分。  
  
使用此新功能有幾個要求和限制︰  
  
-   還原的網域控制站必須能連絡寫入 DC  
  
-   網域中的所有網域控制站必須同時還原  
  
-   來自還原的網域控制站尚未複寫輸出因為掃瞄的任何變更永遠都會遺失  
  
[疑難排解] 區段涵蓋這些案例中，當下列詳細資料確保您不會建立拓撲可能造成問題。  
  
#### <a name="writable-domain-controller-availability"></a>寫入網域控制站可用性  
如果還原，網域控制站必須連接到寫入網域控制站;唯讀模式網域控制站無法傳送 delta 的更新。 拓撲很可能已經，此為寫入網域控制站正確一定需要寫入合作夥伴。 不過，如果還原所有寫入網域控制站同時，都可以找到有效的來源。 相同進入如果寫入網域控制站離線維護或透過網路否則無法存取。  
  
#### <a name="simultaneous-restore"></a>同時還原  
不還原單一網域中的所有網域控制站同時。 如果所有快照都還原一次，Active Directory 複寫運作正常，但 SYSVOL 複寫會停止。 還原的架構 FRS 以及 DFSR 需要他們複本執行個體設為非授權同步模式。 如果所有網域控制站都還原的每個網域控制站標示本身未經授權的 SYSVOL，它們將會再試同步處理群組原則和指令碼的授權夥伴;此時，但，所有合作夥伴也都是未經授權。  
  
> [!IMPORTANT]  
> 還原所有網域控制站同時，如果使用下列文件來設定一個網域控制站-通常肯定-授權，因此的網域控制站傳回正常運作：  
>   
> [使用 BurFlags 登錄鍵初始化檔案複寫服務複本設定](https://support.microsoft.com/kb/290762)  
>   
> [如何將 SYSVOL DFSR 複寫 （例如 「 D4 /d2 」 的 FRS) 的強迫授權和未經授權同步處理](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> 所有網域控制站森林或網域中無法在執行相同的 hypervisor 主機。 導入的每次 hypervisor 離線 cripples AD DS、 換貨，SQL 和其他企業操作失敗單點。 這是不使用只有一個網域控制站的完整網域或樹系不同。 多個網域控制站在多個平台上的協助提供冗餘和容錯。  
  
#### <a name="post-snapshot-replication"></a>後快照複寫  
直到後快照建立已複寫進行輸出原始在本機變更無法還原的快照。 如果其他網域控制站未已經收到這些透過複寫，會需要遺失原始的任何變更。  
  
使用 Repadmin.exe 顯示網域控制站與其夥伴之間任何取消複寫輸出變更：  
  
1.  返回 [DC 的合作夥伴名稱與 DSA 物件 Guid 使用：  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  回到還原網域控制站的合作夥伴網域控制站擱置中輸入的複寫：  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
或者，只是想看到未複寫變更次數：  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
例如 (輸出修改可讀性和重要的項目使用***斜體***)，查看的複寫合作關係 DC4 以下：  
  
```  
C:\>repadmin.exe /showrepl dc4.corp.contoso.com /repsto  
  
Default-First-Site-Name\DC4  
DSA Options: IS_GC  
Site Options: (none)  
DSA object GUID: 5d083398-4bd3-48a4-a80d-fb2ebafb984f  
DSA invocationID: 730fafec-b6d4-4911-88f2-5b64e48fc2f1  
  
==== OUTBOUND NEIGHBORS FOR CHANGE NOTIFICATIONS ============  
  
DC=corp,DC=contoso,DC=com  
    Default-First-Site-Name\DC3 via RPC  
        DSA object GUID: f62978a8-fcf7-40b5-ac00-40aa9c4f5ad3  
        Last attempt @ 2011-11-11 15:04:12 was successful.  
    Default-First-Site-Name\DC2 via RPC  
        DSA object GUID: 3019137e-d223-4b62-baaa-e241a0c46a11  
        Last attempt @ 2011-11-11 15:04:15 was successful.  
```  
  
現在，您知道它已複寫 DC2 與 DC3。 然後顯示變更清單，DC2 聲明仍然不已從 DC4，然後查看一個新的群組：  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com  
  
==== SOURCE DSA: (null) ====  
Objects returned: 1  
(0) add CN=newgroup4,CN=Users,DC=corp,DC=contoso,DC=com  
    1> parentGUID: 55fc995a-04f4-4774-b076-d6a48ac1af99  
    1> objectGUID: 96b848a2-df1d-433c-a645-956cfbf44086  
    2> objectClass: top; group  
    1> instanceType: 0x4 = ( WRITE )  
    1> whenCreated: 11/11/2011 3:03:57 PM Eastern Standard Time  
```  
  
您也想測試以確定它已不已複寫的其他合作夥伴。  
  
或者，如果不是由您最不需要複製的物件，只有在意這次的任何物件已未完成，您可以使用**/statistics**選項：  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> 測試所有寫入合作夥伴，如果您看到的任何失敗或複寫未完成。 只要至少會聚集，是一般放心地還原快照，如轉移複寫最後協調其他伺服器。  
>   
> 請務必注意複寫顯示 /showchanges 中的任何錯誤，並不會繼續他們的修正。  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Windows PowerShell 快照 Cmdlet  
Windows PowerShell HYPER-V 下列模組 cmdlet 提供快照功能在 Windows Server 2012 中：  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  


