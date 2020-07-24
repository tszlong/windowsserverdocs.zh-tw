---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: 虛擬網域控制站的部署與設定
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 88f4f501e0bfd173d91a55ee60f226482139b6b2
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963210"
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>虛擬網域控制站的部署與設定

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題涵蓋︰  
  
-   [安裝考慮](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    本節包括平台需求和其他重要限制。  
  
-   [虛擬網域控制站複製](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    本節將詳細說明整個虛擬網域控制器的複製程序。  
  
-   [安全還原虛擬網域控制站](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    本節將詳細說明在安全還原虛擬網域控制站期間所做的驗證。  
  
## <a name="installation-considerations"></a><a name="BKMK_InstallConsiderations"></a>安裝考慮  
沒有任何適用於虛擬網域控制站的特殊角色或功能安裝；所有的網域控制站都會自動包含複製與安全還原功能。 您無法移除或停用這些功能。  
  
使用 Windows Server 2012 網域控制站需要 Windows Server 2012 AD DS 結構描述版本 56 或更高版本，以及相當於 Windows Server 2003 原生或更高版本的樹系功能等級。  
  
可寫入和唯讀的網域控制站都支援虛擬 DC 的所有層面，就像使用通用類別目錄和 FSMO 角色一樣。  
  
> [!IMPORTANT]  
> 當複製開始時，PDC 模擬器 FSMO 角色持有者必須位於線上。  
  
### <a name="platform-requirements"></a><a name="BKMK_PlatformReqs"></a>平臺需求  
複製虛擬網域控制站需要：  
  
-   裝載於 Windows Server 2012 DC 上的 PDC 模擬器 FSMO 角色  
  
-   可以在複製作業期間使用的 PDC 模擬器  
  
複製與安全還原都需要：  
  
-   Windows Server 2012 虛擬化的客體  
  
-   虛擬化主機平台支援 VM 世代識別碼 (VMGID)  
  
檢閱下表中的虛擬化產品，以及它們是否支援虛擬網域控制站和 VM 世代識別碼。  
  
|||  
|-|-|  
|**虛擬化產品**|**支援虛擬網域控制站和 VMGID**|  
|**Microsoft Windows Server 2012 伺服器 (具備 Hyper-V 功能)**|是|  
|**Microsoft Windows Server 2012 Hyper-V Server**|是|  
|**Microsoft Windows 8 (具備 Hyper-V 用戶端功能)**|是|  
|**Windows Server 2008 R2 和 Windows Server 2008**|否|  
|**非 Microsoft 的虛擬化解決方案**|請連絡廠商|  
  
即使 Microsoft 支援 Windows 7 Virtual PC、Virtual PC 2007、Virtual PC 2004 及 Virtual Server 2005，它們還是無法執行 64 位元的客體，也不支援-VM 世代識別碼。  
  
如需協力廠商虛擬化產品的說明及其支援虛擬網域控制站的方針，請直接與該廠商連絡。  
  
如需詳細資訊，請檢閱[Microsoft software running in non-Microsoft hardware virtualization software](https://support.microsoft.com/kb/897615) (在非 Microsoft 硬體虛擬化軟體中執行的 Microsoft 軟體) 的支援政策。  
  
### <a name="critical-caveats"></a>重要注意事項  
虛擬網域控制站「不」** 支援下列項目的安全還原：  
  
-   手動複製 VHD 和 VHDX 檔案以覆寫現有的 VHD 檔案  
  
-   使用檔案備份或完整磁碟備份軟體來還原 VHD 和 VHDX 檔案  
  
> [!NOTE]  
> VHDX 檔案是 Windows Server 2012 Hyper-V 中的新功能。  
  
這些作業都不具有 VM-GenerationID 的意義，因此不會變更 VM 世代識別碼。 使用這些方法還原網域控制站，可能會導致 USN 復原，也可能會隔離網域控制站或引進延遲物件，並且需要進行整個樹系的清除作業。  
  
> [!WARNING]  
> 虛擬網域控制站的安全還原不能取代系統狀態備份和 AD DS 資源回收筒。  
>   
> 還原快照之後，在快照後源自於該網域控制站之先前未複寫的變更差異將永久遺失。 安全還原會實作自動非權威還原，「僅能」** 防止意外發生網域控制站隔離。  
  
如需 USN 泡泡與延遲物件的詳細資訊，請參閱[Troubleshooting Active Directory operations that fail with error 8606: "Insufficient attributes were given to create an object"](https://support.microsoft.com/kb/2028495) (為 Active Directory 作業失敗進行疑難排解，出現錯誤 8606：「提供的屬性不足以建立物件」)。  
  
## <a name="virtualized-domain-controller-cloning"></a><a name="BKMK_VDCCloning"></a>虛擬網域控制站複製  
不論使用的是圖形工具或 Windows PowerShell，複製虛擬網域控制站都有許多階段和步驟。 可概略分為下列三個階段：  
  
**準備環境**  
  
-   步驟 1：驗證 Hypervisor 是否支援 VM 世代識別碼，且可進行複製  
  
-   步驟2：確認 PDC 模擬器角色是由執行 Windows Server 2012 的網域控制站主控，且在複製期間已上線，而且已由複製的網域控制站連線。  
  
**準備來源網域控制站**  
  
-   步驟 3：授權來源網域控制站進行複製  
  
-   步驟 4：移除不相容的服務或程式，或將其加入 CustomDCCloneAllowList.xml 檔案中。  
  
-   步驟 5：建立 DCCloneConfig.xml  
  
-   步驟 6：使來源網域控制站離線  
  
**建立複製的網域控制站**  
  
-   步驟 7：複製或匯出來源 VM，並新增 XML (若尚未複製)  
  
-   步驟 8：從複本建立新的虛擬機器  
  
-   步驟 9：啟動新的虛擬機器以開始複製  
  
使用圖形工具 (例如 Hyper-V 管理主控台) 或命令列工具 (例如 Windows PowerShell) 時，在操作中並無任何程序差異，因此，只會呈現這些步驟一次來說明這兩個介面。 本主題提供 Windows PowerShell 範例，讓您能夠探索複製程序的完整的自動化過程；它們對於任何步驟而言都不是必要的。 Windows Server 2012 中並未隨附任何適用於虛擬網域控制站的圖形管理工具。  
  
在程序中有數個位置可讓您選擇如何建立複製的電腦，以及如何新增 xml 檔案；以下將詳述這些步驟。 除此之外，這稱得上是不變的程序。  
  
下圖說明虛擬網域控制站的複製程序，而網域已經存在其中。  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>步驟 1 - 驗證 Hypervisor  
檢閱廠商文件，以確定來源網域控制站正在支援的 Hypervisor 上執行。 虛擬網域控制站與 Hypervisor 無關，不需要 Hyper-V。  
  
如果虛擬程式是 Microsoft Hyper-v，請確定它正在 Windows Server 2012 上執行。 您可以使用 [裝置管理] 來驗證這一點  
  
開啟 [Devmgmt.msc]****，然後針對已安裝的 Microsoft Hyper-V 裝置和驅動程式檢查 [系統裝置]****。 虛擬網域控制站所需的特定系統裝置是**Microsoft Hyper-v 世代計數器**（驅動程式： vmgencounter.sys）。  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>步驟 2 - 確認 PDCE FSMO 角色  
在您嘗試複製 DC 之前，必須先驗證裝載網域主控站模擬器 FSMO 的網域控制站執行的是 Windows Server 2012。 需要 PDC 模擬器 (PDCE) 有下列數個原因：  
  
1.  PDCE 會建立特殊的 [可複製的網域控制站]**** 群組，並在網域的根目錄上設定它的權限，以允許網域控制站複製其本身。  
  
2.  複製網域控制站會使用 DRSUAPI RPC 通訊協定直接連絡 PDCE，以便為複製 DC 建立電腦物件。  
  
    > [!NOTE]  
    > Windows Server 2012 擴充了現有的目錄複寫服務 (DRS) 遠端通訊協定 (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**) 以納入新的 RPC 方法 **IDL_DRSAddCloneDC** (Opnum **28**)。 **IDL_DRSAddCloneDC** 方法會藉由從現有的網域控制站物件複製屬性，來建立新的網域控制站物件。  
    >   
    > 網域控制站的狀態是由電腦、伺服器、NTDS 設定、FRS、DFSR，以及針對每個網域控制站所維護的連線物件所組成。 複製物件時，這個 RPC 方法會使用新網域控制站的對應物件，來取代原始網域控制站的所有參考。 呼叫者在網域命名內容上必須具備控制存取權限 DS-Clone-Domain-Controller。  
    >   
    > 使用這個新方法，一律需要從呼叫者直接存取 PDC 模擬器網域控制站。  
    >   
    > 因為這個 RPC 方法是新的，所以，您的網路分析軟體需要更新的剖析器，以包含適用於現有 UUID E3514235-4B06-11D1-AB04-00C04FC2DCD2 中的新 Opnum 28 欄位。 否則，就無法剖析這個流量。  
    >   
    > 如需詳細資訊，請參閱 [4.1.29 IDL_DRSAddCloneDC (Opnum 28)](/openspecs/windows_protocols/ms-drsr/ef0bfb1d-037b-4626-a6d9-cc7589bc5786)。  
  
***這也表示，使用非完全路由的網路時，複製虛擬網域控制站需要具備存取 PDCE 權限的網路區段***。 您可以在複製之後將複製的網域控制站移至不同的網路 (就像是實體網域控制站一樣)，只要您謹慎更新 AD DS 邏輯站台資訊即可。  
  
> [!IMPORTANT]  
> 在複製僅包含單一網域控制站的網域時，您必須先確定已使來源 DC 再度上線，然後才能開始複製複本。 實際執行的網域一律至少應包含兩個網域控制站。  
  
#### <a name="active-directory-users-and-computers-method"></a>Active Directory 使用者和電腦方法  
  
1.  使用 Dsa.msc 嵌入式管理單元、在網域上按一下滑鼠右鍵，然後按一下 [操作主機]****。 記下 [PDC] 索引標籤上具名的網域控制站，然後關閉對話方塊。  
  
2.  在 DC 的電腦物件上按一下滑鼠右鍵、按一下 [屬性]****，然後驗證 [作業系統] 資訊。  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
您可以結合下列的 Active Directory Windows PowerShell 模組 Cmdlet，以傳回 PDC 模擬器的版本：  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
如果未提供網域，這些 Cmdlet 會假設執行電腦的網域。  
  
下列命令會傳回 PDCE 和 [作業系統] 資訊：  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
以下範例示範如何指定網域名稱，以及在 Windows PowerShell 管線之前篩選傳回的屬性：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>步驟 3 - 授權來源 DC  
來源網域控制站在網域 NC 標頭上必須具備控制存取權限 (CAR) [允許 DC 建立本身的複製品]****。 根據預設，已知群組 [可複製的網域控制站]**** 具備這個權限且未含任何成員。 PDCE 會在該 FSMO 角色移轉到 Windows Server 2012 網域控制站時建立這個群組。  
  
#### <a name="active-directory-administrative-center-method"></a>Active Directory 管理中心方法  
  
1.  啟動 Dsac.exe 並瀏覽到來源 DC，然後開啟它的詳細資料頁面。  
  
2.  在 [成員隸屬]**** 區段中，新增該網域的 [可複製的網域控制站]**** 群組。  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
您可以結合下列的 Active Directory Windows PowerShell 模組 Cmdlet **get-adcomputer** 和 **add-adgroupmember**，將網域控制站新增到 [可複製的網域控制站]**** 群組：  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
例如，這會將伺服器 DC1 新增到群組，而不需指定群組成員的辨別名稱：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>重建預設權限  
如果您從網域標頭移除這個權限，複製即會失敗。 您可以使用 Active Directory 管理中心或 Windows PowerShell 重新建立權限。  
  
##### <a name="active-directory-administrative-center-method"></a>Active Directory 管理中心方法  
  
1.  開啟 [Active Directory 管理中心]****、在網域標頭上按一下滑鼠右鍵，依序按一下 [屬性]****、[擴充功能]**** 索引標籤、[安全性]**** 及 [進階]****。 按一下 [只有這個物件]****。  
  
2.  按一下 [新增]****，在 [輸入要選取的物件名稱]**** 下方，輸入群組名稱 [可複製的網域控制站]****。  
  
3.  在 [權限] 下方，按一下 [允許 DC 建立本身的複製品]****，然後按一下 [確定]****。  
  
> [!NOTE]  
> 您也可以移除預設的權限，並新增個別的網域控制站。 這樣做很可能會導致持續發生維護問題，而新的系統管理員將不知道這項自訂。 變更預設設定不會提高安全性，因此不建議使用。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
以提升權限的系統管理員身分，在 Windows PowerShell 主控台提示字元中使用下列命令。 這些命令會偵測網域名稱，並以預設權限再次新增：  
  
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
  
或者，在 Windows PowerShell 主控台中執行範例 [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)，其中的主控台會在受影響的網域中的網域控制站上，以提升權限的系統管理員身分啟動。 它會自動設定權限。 這個範例位於本單元的附錄中。  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>步驟 4 - 移除不相容的應用程式或服務 (如果不使用 CustomDCCloneAllowList.xml)  
Get-ADDCCloningExcludedApplicationList 先前傳回「且未新增到 CustomDCCloneAllowList.xml」** 的任何程式或服務都必須在複製之前移除。 建議的方法是取消安裝應用程式或服務。  
  
> [!WARNING]  
> 未取消安裝或新增到 CustomDCCloneAllowList.xml 的所有不相容的程式或服務都會妨礙複製進行。  
  
使用 Get-AdComputerServiceAccount Cmdlet 來尋找網域中任何獨立的受管理服務帳戶 (MSA)，以及這台電腦是否正在使用它們其中任何一個。 如果已安裝任一個 MSA，請使用 Uninstall-ADServiceAccount Cmdlet 來移除本機安裝的服務帳戶。 當您在步驟 6 中完成使來源網域控制站離線的作業之後，就可以在使伺服器再度上線時，使用 Install-ADServiceAccount 重新新增 MSA。 如需詳細資訊，請參閱 [Uninstall-ADServiceAccount](/openspecs/windows_protocols/ms-drsr/ef0bfb1d-037b-4626-a6d9-cc7589bc5786)。  
  
> [!IMPORTANT]  
> 獨立的 MSA (在 Windows Server 2008 R2 中首次發行) 已在 Windows Server 2012 中使用群組 MSA 來取代。 群組 MSA 支援複製。  
  
### <a name="step-5---create-dccloneconfigxml"></a>步驟 5 - 建立 DCCloneConfig.xml  
需要有 DcCloneConfig.xml 檔案，才能複製網域控制站。 它的內容讓您能夠指定像是新電腦名稱和 IP 位址等唯一的詳細資料。  
  
除非您在來源網域控制站上安裝應用程式或可能不相容的 Windows 服務，否則 CustomDCCloneAllowList.xml 檔案是選用的。 檔案需要精確的命名、設定格式及位置；否則，複製將會失敗。  
  
基於這個原因，您應該一律使用 Windows PowerShell Cmdlet 來建立 XML 檔案，並將它們放置於正確的位置。  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>使用 New-ADDCCloneConfigFile 來產生  
Active Directory Windows PowerShell 模組在 Windows Server 2012 中包含新的 Cmdlet：  
  
```  
New-ADDCCloneConfigFile  
```  
  
您可以在想要進行複製的來源網域控制站上執行這個 Cmdlet。 這個 Cmdlet 支援多個引數，除非您指定了 -offline 引數，否則，在使用時一律會測試執行它的電腦和環境。  
  
||||  
|-|-|-|  
|**ActiveDirectory**<p>**Cmdlet**|**引數**|**說明**|  
|**New-ADDCCloneConfigFile**|*<no argument specified>*|在 DSA 工作目錄 (預設為 %systemroot%\ntds) 中建立空白的 DcCloneConfig.xml 檔案|  
||-CloneComputerName|指定複製 DC 電腦名稱。 字串資料類型。|  
||-Path|指定要建立 DcCloneConfig.xml 的資料夾。 若未指定，則會寫入 DSA 工作目錄 (預設為 %systemroot%\ntds)。 字串資料類型。|  
||-SiteName|指定要在複製電腦帳戶建立期間加入的 AD 邏輯站台名稱。 字串資料類型。|  
||-IPv4Address|指定複製電腦的靜態 IPv4 位址。 字串資料類型。|  
||-IPv4SubnetMask|指定複製電腦的靜態 IPv4 子網路遮罩。 字串資料類型。|  
||-IPv4DefaultGateway|指定複製電腦的靜態 IPv4 預設閘道位址。 字串資料類型。|  
||-IPv4DNSResolver|在以逗號分隔的清單中，指定複製電腦的靜態 IPv4 DNS 項目。 陣列資料類型。 最多可提供四個項目。|  
||-PreferredWINSServer|指定主要 WINS 伺服器的靜態 IPv4 位址。 字串資料類型。|  
||-AlternateWINSServer|指定次要 WINS 伺服器的靜態 IPv4 位址。 字串資料類型。|  
||-IPv6DNSResolver|在以逗號分隔的清單中，指定複製電腦的靜態 IPv6 DNS 項目。 沒有任何方法可以在複製虛擬網域控制站時設定 IPv6 靜態資訊。 陣列資料類型。|  
||-Offline|不會執行驗證測試，並覆寫任何現有的 dccloneconfig.xml。 沒有參數。|  
||*-靜態*|如果指定了靜態 IP 引數 IPv4SubnetMask、IPv4SubnetMask 或 IPv4DefaultGateway，即為必要引數。 沒有參數。|  
  
在線上模式中執行時執行的測試：  
  
-   PDC 模擬器是 Windows Server 2012 或更新版本  
  
-   來源網域控制站是 [可複製網域控制站] 群組的成員  
  
-   來源網域控制站不包含任何排除的應用程式或服務  
  
-   來源網域控制站在指定的路徑中已經沒有包含 DcCloneConfig.xml  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>步驟 6 - 使來源網域控制站離線  
您無法複製執行中的來源 DC；必須讓它順利關機。 不要複製經由強制關閉電源而停止的網域控制站。  
  
#### <a name="graphical-method"></a>圖形方式  
使用執行中 DC 內的關機按鈕，或者 [Hyper-V 管理員] 的關機按鈕。  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
您可以使用下列任一個 Cmdlet 來將虛擬機器關機：  
  
```  
Stop-computer  
Stop-vm  
```  
  
Stop-computer 是不論是否虛擬化，都支援將電腦關機的 Cmdlet，類似傳統的 Shutdown.exe 公用程式。 Stop-vm 是 Windows Server 2012 Hyper-V Windows PowerShell 模組中新的 Cmdlet，相當於 [Hyper-V 管理員] 中的電源選項。 實驗室環境中的網域控制站通常是在私人虛擬網路上進行，因此後者非常有用。  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>步驟 7 - 複製磁碟  
複製階段中需要有系統管理選項：  
  
-   手動複製磁碟，而不使用 Hyper-V  
  
-   使用 Hyper-V 匯出 VM  
  
-   使用 Hyper-V 匯出合併的磁碟  
  
必須複製虛擬機器的所有磁碟，不只是系統磁碟機。 如果來源網域控制站使用差異磁碟，而您規劃將複製的網域控制站移到另一台 HYPER-V 主機，則您必須將它匯出。  
  
如果來源網域控制站只有「一個」** 磁碟機，則建議手動複製磁碟。 建議針對含有「一個以上」** 磁碟機或其他複雜的虛擬硬體自訂 (例如多個 NIC) 的 VM 使用匯出/匯入。  
  
如果手動複製檔案，請在複製前先刪除所有快照。 如果匯出 VM，請在匯出前先刪除快照，或者在匯入後從新的 VM 中刪除它們。  
  
> [!WARNING]  
> 快照是可使網域控制站返回先前狀態的差異磁碟。 如果您是複製網域控制站，然後還原其複製前的快照，則您的樹系中最後會出現重複的網域控制站。 在新複製的網路控制站上，先前的快照中沒有任何值。  
  
#### <a name="manually-copying-disks"></a>手動複製磁碟  
  
##### <a name="hyper-v-manager-method"></a>HYPER-V 管理員方法  
使用 [Hyper-V 管理員] 嵌入式管理單元，來判斷與來源網域控制站關聯的磁碟。 使用 [檢查] 選項，驗證網域控制站是否使用差異磁碟 (這需要您同時複製父磁碟)  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
若要刪除快照，請選取 VM，然後刪除快照樹狀子目錄。  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
然後您可以使用 Windows 檔案總管、Xcopy.exe 或 Robocopy.exe，手動複製 VHD 或 VHDX 檔案。 不需要任何特殊步驟。 最好的做法是變更檔名，即使移到其他資料夾也一樣。  
  
> [!NOTE]  
> 如果在 LAN 上 (1 Gbit 或以上) 的主機電腦之間進行複製，**Xcopy.exe /J** 選項複製 VHD/VHDX 檔案的速度會大幅領先任何其他工具，代價則是需要更大的頻寬使用量。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要使用 Windows PowerShell 來判斷磁碟，請使用 HYPER-V 模組：  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
例如，您可以利用下列範例，從名為 **DC2** 的 VM 傳回所有的 IDE 硬碟：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
如果磁碟路徑指向 AVHD 或 AVHDX 檔案，它就是快照。 若要刪除與磁碟關聯的快照並與實際的 VHD 或 VHDX 合併，請使用下列 Cmdlet：  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
例如，若要從名為 DC2-SOURCECLONE 的 VM 刪除所有快照：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
若要使用 Windows PowerShell 來複製檔案，請使用下列 Cmdlet：  
  
```  
Copy-Item  
```  
  
在管線中合併 VM Cmdlet 以協助自動化。 管線是用來在多個 Cmdlet 之間傳送資料的通道。 例如，若要將名為 DC2-SOURCECLONE 的離線來源網域控制站的磁碟機複製到名為 c:\temp\copy.vhd 的新磁碟，而不需知道其系統磁碟機的確切路徑：  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> 您無法使用傳遞磁碟來複製，因為它們不使用虛擬磁碟檔案，而是使用實際的硬碟。  
  
> [!NOTE]  
> 如需更多 Windows PowerShell 作業搭配管線的詳細資訊，請參閱 [Windows PowerShell 中的管線處理與管線](/previous-versions/windows/it-pro/windows-powershell-1.0/ee176927(v=technet.10))。  
  
#### <a name="exporting-the-vm"></a>匯出 VM  
複製磁碟的另一種方式是匯出整個 Hyper-V VM 做為複本。 自動匯出會建立針對 VM 命名的資料夾，並包含所有的磁碟和設定資訊。  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>HYPER-V 管理員方法  
使用 [HYPER-V 管理員] 匯出 VM：  
  
1.  在來源網域控制站上按一下滑鼠右鍵，然後按一下 [匯出]****。  
  
2.  選取現有的資料夾做為匯出容器。  
  
3.  等待 [狀態] 欄停止顯示 [正在匯出]****。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要使用 HYPER-V Windows PowerShell 模組匯出 VM，請使用下列 Cmdlet：  
  
```  
Export-vm  
```  
  
例如，若要將名為 DC2-SOURCECLONE 的 VM 匯出到名為 C:\VM 的資料夾：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> Windows Server 2012 Hyper-V 支援新的匯出和匯入功能 (這些功能已超出本訓練的範圍)。 如需詳細資訊，請檢閱 TechNet。  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>使用 Hyper-V 匯出合併的磁碟  
最後一個選擇是使用 Hyper-V 中的 [磁碟合併] 與 [轉換] 選項。 這些選項可讓您製作現有磁碟結構的複本 (即使在包含快照集 AVHD/AVHDX 檔案時也一樣)，使其成為單一的新磁碟。 就像手動磁碟複製案例，這主要是針對只使用單一磁片磁碟機的較簡單虛擬機器，例如 C： \\ 。 它的唯一優點是不需要先刪除快照，這是它與手動複製的不同。 這個操作的速度必然會比只刪除快照並複製磁碟的程序還要慢。  
  
##### <a name="hyper-v-manager-method"></a>HYPER-V 管理員方法  
使用 [HYPER-V 管理員] 建立合併的磁碟：  
  
1.  按一下 [**編輯磁片**]。  
  
2.  瀏覽最低階的子磁碟。 例如，如果您使用的是差異磁碟，子磁碟就是最低階的子項。 如果虛擬機器有一個 (或多個) 快照，則目前選取的快照就是最低階的子磁碟。  
  
3.  選取 [合併]**** 選項，為整個父系-子系結構建立單一磁碟。  
  
4.  選取新的虛擬硬碟，並提供路徑。 這會將現有的 VHD/VHDX 檔案調解成為新的單一可攜式單位，還原先前快照時便不會有風險。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
若要使用 HYPER-V Windows PowerShell 模組從一組複雜的父項建立合併磁碟，請使用下列 Cmdlet：  
  
```  
Convert-vm  
```  
  
例如，若要將整個系列的 VM 磁碟快照 (這次不包括任何差異磁碟) 以及父磁碟匯出到名為 DC4-CLONED.VHDX 的新單一磁碟：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="adding-xml-to-the-offline-system-disk"></a><a name="BKMK_Offline"></a>將 XML 新增到離線系統磁碟  
如果您將 Dccloneconfig.xml 複製到執行中的來源 DC，現在就必須將更新的 dccloneconfig.xml 檔案複製到離線複製/匯出的系統磁碟中。 根據先前使用 Get-ADDCCloningExcludedApplicationList 偵測到的已安裝應用程式而定，您可能也需要將 CustomDCCloneAllowList.xml 檔案複製到磁碟。  
  
下列位置可以包含 DcCloneConfig.xml 檔案：  
  
1.  DSA 工作目錄  
  
2.  %windir%\NTDS  
  
3.  依磁碟機代號順序的卸除式讀/寫媒體，位於磁碟機的根目錄  
  
您無法設定這些路徑。 複製開始之後，複製會使用該特定順序來檢查這些位置，並使用找到的第一個 DcCloneConfig.xml 檔案，而不論其他資料夾的內容為何。  
  
下列位置可以包含 CustomDCCloneAllowList.xml 檔案：  
  
1.  HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters  
  
    AllowListFolder (*REG_SZ*)  
  
2.  DSA 工作目錄  
  
3.  %windir%\NTDS  
  
4.  依磁碟機代號順序的卸除式讀/寫媒體，位於磁碟機的根目錄  
  
您可以使用 New-ADDCCloneConfigFile 搭配 **-offline** 引數來執行 (也稱為離線模式)，以建立 DcCloneConfig.xml 檔案，並將它放入正確的位置。 下列範例示範如何在離線模式中執行 New-ADDCCloneConfigFile。  
  
若要在離線模式中建立名為 CloneDC1 的複製網域控制站，請在名為「REDMOND」的網站中，使用靜態 IPv4 位址輸入：  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
若要在離線模式中建立名稱為 Clone2 且具有靜態 IPv4 和靜態 IPv6 設定的複製網域控制站，請輸入：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
若要在離線模式中建立具有靜態 IPv4 和動態 IPv6 設定的複製網域控制站，並為 DNS 解讀器設定指定多個 DNS 伺服器，請輸入：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
若要在離線模式中建立名稱為 Clone1 且具有動態 IPv4 和靜態 IPv6 設定的複製網域控制站，請輸入：  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
若要在離線模式中建立具有動態 IPv4 和動態 IPv6 設定的複製網域控制站，請輸入：  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Windows 檔案總管方法  
Windows Server 2012 現在提供掛接 VHD 和 VHDX 檔案的圖形化選項。 這需要在 Windows Server 2012 上安裝 [桌面體驗] 功能。  
  
1.  按一下新複製且包含來源 DC 系統磁碟機或 DSA 工作目錄位置資料夾的 VHD/VHDX 檔案，然後按一下 [光碟映像工具]**** 功能表中的 [掛接]****。  
  
2.  在目前掛接的磁碟機中，將 XML 檔案複製到有效的位置。 系統可能會提示您具備資料夾的權限。  
  
3.  按一下掛接的磁碟機，然後按一下 [磁碟工具]**** 功能表中的 [退出]****。  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVClickMountedDrive.png)  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDetailsMountedDrive.gif)  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVEjectMountedDrive.gif)  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
或者，您可以掛接離線磁碟，並使用 Windows PowerShell Cmdlet 複製 XML 檔案：  
  
```  
mount-vhd  
get-disk  
get-partition  
get-volume  
Add-PartitionAccessPath  
Copy-Item  
```  
  
這讓您能夠擁有該程序的完整控制權。 例如，可以使用特定磁碟機代號來掛接磁碟機、複製檔案，以及卸載磁碟機。  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
例如：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
或者，您可以使用新 **Mount-DiskImage** Cmdlet 來掛接 VHD (或 ISO) 檔案。  
  
### <a name="step-8---create-the-new-virtual-machine"></a>步驟 8 - 建立新的虛擬機器  
開始複製程序前的最後設定步驟是，建立新的 VM 來使用複製來源網域控制站的磁碟。 根據您在複製磁碟階段所做的選項，您會有兩個選項：  
  
1.  將新的 VM 關聯到複製的磁碟  
  
2.  匯入匯出的 VM  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>將新的 VM 關聯到複製的磁碟  
如果您手動複製系統磁碟，就必須使用複製的磁碟來建立新的虛擬機器。 Hypervisor 會在建立新的 VM 時自動設定 VM 世代識別碼；不需要在 VM 或 Hyper-V 主機上進行任何設定變更。  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>HYPER-V 管理員方法  
  
1.  建立新的虛擬機器。  
  
2.  指定 VM 名稱、記憶體及網路。  
  
3.  在 [連接虛擬硬碟] 頁面上，指定複製的系統磁碟。  
  
4.  完成精靈以建立 VM。  
  
如果有多個磁碟、網路介面卡或其他自訂項目，請在啟動網域控制站之前先設定它們。 建議針對複雜的 VM 使用「匯出匯入」方法來複製磁碟。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
您可以使用 HYPER-V Windows PowerShell 模組，使用下列 Cmdlet 在 Windows Server 2012 中自動建立 VM：  
  
```  
New-VM  
```  
  
例如，在此處建立 DC4-CLONEDFROMDC2 VM，使用 1GB 的 RAM，從 c:\vm\dc4-systemdrive-clonedfromdc2.vhd 檔案開機，並使用 10.0 虛擬網路：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>匯入 VM  
如果您先前已匯出 VM，現在需要再次以複本形式匯入。 這會使用匯出的 XML，利用所有先前的設定、磁碟機、網路及記憶體設定來重新建立電腦。  
  
如果您想要從相同的匯出 VM 建立其他複本，可視需要製作多份匯出 VM 的複本。 然後針對每個複本使用 [匯入]。  
  
> [!IMPORTANT]  
> 請務必使用 [複製]**** 選項，因為匯出保留所有來源資訊；如果在相同的 HYPER-V 主機伺服器上執行，使用 [移動]**** 或 [就地]**** 匯入伺服器會造成資訊衝突。  
  
##### <a name="hyper-v-manager-method"></a>HYPER-V 管理員方法  
使用 [HYPER-V 管理員] 嵌入式管理單元進行匯入：  
  
1.  按一下 [匯入虛擬機器]****  
  
2.  在 [尋找資料夾]**** 頁面上，使用 [瀏覽] 按鈕選取匯出的 VM 定義檔案  
  
3.  在 [選取虛擬機器]**** 頁面上，按一下來源電腦。  
  
4.  在 [選擇匯入類型]**** 頁面上，按一下 [複製虛擬機器 (建立新的唯一識別碼)]****，然後按一下 [完成]****。  
  
5.  如果是在相同的 Hyper-V 主機上進行匯入，請將匯入的 VM 重新命名；它的名稱將與匯出的來源網域控制站相同。  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
請記得使用 [Hyper-V 管理] 嵌入式管理單元來移除任何匯入的快照：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> 請務必刪除任何匯入的快照；如果套用，它們就會使複製的網域控制站回到先前 (而且可能是即時的) DC 的狀態，導致複寫失敗、重複的 IP 資訊，以及其他干擾。  
  
##### <a name="windows-powershell-method"></a>Windows PowerShell 方法  
您可以使用 HYPER-V Windows PowerShell 模組，使用下列 Cmdlet 在 Windows Server 2012 中自動匯入 VM：  
  
```  
Import-VM  
Rename-VM  
```  
  
例如，此處是使用其自動判斷的 XML 檔案來匯入匯出的 VM DC2-CLONED，然後立即重新命名為新的 VM 名稱 DC5-CLONEDFROMDC2：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
請記得使用下列 Cmdlet 來移除任何匯入的快照：  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
例如：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]
> 確定在匯入電腦時，不會將靜態 MAC 位址指派給來源網域控制站。 如果複製的來源電腦具有靜態 MAC，這些複製的電腦將不會正確傳送或接收任何網路流量。 如果是這種情況，請設定新的唯一靜態或動態 MAC 位址。 您可以使用下列命令來查看 VM 是否使用靜態 MAC 位址：  
> 
> **取得-VM-VMName**   
>  ***測試-vm* |VMNetworkAdapter |佛羅里達\\***  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>步驟 9 - 複製新的虛擬機器  
(選擇性) 開始複製之前，請重新啟動離線複製來源網域控制站。 無論如何，請確定 PDC 模擬器已在線上。  
  
若要開始複製，只需啟動新的虛擬機器即可。 此程序會自動初始化，而網域控制站會在複製完成之後自動重新開機。  
  
> [!IMPORTANT]  
> 不建議讓網域控制站維持關閉狀態一段很長的時間，而且如果將複製品加入與其來源 DC 相同的站台，若來源網域控制站離線，則最初的站台內和站台間複寫拓撲可能需要較長的時間來建立。  
  
如果使用 Windows PowerShell 來啟動 VM，新的 HYPER-V 模組 Cmdlet 如下：  
  
```  
Start-VM  
```  
  
例如：  
  
![虛擬化 DC 部署](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
當電腦在複製完成後重新啟動之後，它就是一個網域控制站，而您可以正常登入以確認正常運作。 如果發生任何錯誤，伺服器設定為開機進入 [目錄服務還原模式] 以供進行調查。  
  
## <a name="virtualization-safeguards"></a><a name="BKMK_VDCSafeRestore"></a>虛擬化保護措施  
與複製虛擬網域控制站不同，Windows Server 2012 虛擬化保護措施不需要任何設定步驟。 只要您符合一些簡單條件，這個功能就能在沒有任何介入的情況下運作：  
  
-   Hypervisor 支援 VM 世代識別碼  
  
-   有一個有效的夥伴網域控制站，還原的網域控制站可進行非權威複寫變更。  
  
### <a name="validate-the-hypervisor"></a>驗證 Hypervisor  
檢閱廠商文件，以確定來源網域控制站正在支援的 Hypervisor 上執行。 虛擬網域控制站與 Hypervisor 無關，不需要 Hyper-V。  
  
檢閱先前的[平台需求](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs)一節，了解已知的 VM 世代識別碼支援。  
  
如果您要從來源 Hypervisor 將 VM 移轉到不同的目標 Hypervisor，根據 Hypervisor 是否支援 VM 世代識別碼而定 (如下表所述)，不一定會觸發虛擬化保護措施。  
  
|來源 Hypervisor|目標 Hypervisor|結果|  
|---------------------|---------------------|----------|  
|支援 VM 世代識別碼|不支援 VM 世代識別碼|不會觸發保護措施 (如果 DCCloneConfigFile.xml 存在，DC 將會開機進入 DSRM)|  
|不支援 VM 世代識別碼|支援 VM 世代識別碼|觸發保護措施|  
|支援 VM 世代識別碼|支援 VM 世代識別碼|不會觸發保護措施，因為 VM 定義並未變更，這表示 VM 世代識別碼維持不變|  
  
### <a name="validate-the-replication-topology"></a>驗證複寫拓撲  
虛擬化保護措施會針對 Active Directory 複寫的差異初始化非權威輸入複寫，並以非權威方式重新同步處理所有的 SYSVOL 內容。 這樣可確保網域控制站從快照返回時具備完整功能，最終與環境中的其他部分保持一致。  
  
使用這個新功能有數個需求和限制：  
  
-   還原的網域控制站必須能夠連絡可寫入的 DC  
  
-   不得同時還原網域中的所有網域控制站  
  
-   所有源自還原的網域控制站，但在取得快照之後尚未複寫輸出的變更將會永久遺失。  
  
儘管疑難排解章節會涵蓋這些案例，但是下列的詳細資料可確保您不會建立可能導致問題的拓撲。  
  
#### <a name="writable-domain-controller-availability"></a>可寫入網域控制站的可用性  
如果已還原，網域控制站就必須連線到可寫入的網域控制站；唯讀的網域控制站無法傳送更新的差異。 拓撲很可能已經正確，因為可寫入的網域控制站永遠都需要可寫入的協力電腦。 不過，如果同時還原所有可寫入的網域控制站，便全都找不到有效的來源。 如果可寫入的網域控制站已離線，或是無法透過網路連線，則會出現相同情況。  
  
#### <a name="simultaneous-restore"></a>同時還原  
請勿在單一網域中同時還原所有的網域控制站。 如果同時還原所有快照，Active Directory 複寫會正常運作，但是 SYSVOL 複寫會停止。 FRS 和 DFSR 的還原架構要求將它們的複本執行個體設定為非權威同步處理模式。 如果同時還原所有網域控制站，而且每一個網域控制站都針對 SYSVOL 將其本身標記為非權威狀態，則它們全都將嘗試同步處理來自權威協力電腦的群組原則和指令碼；不過，此時所有的協力電腦也都是非權威的。  
  
> [!IMPORTANT]  
> 如果同時還原所有的網域控制站，請使用下列文章，將某一個網域控制站 (通常是 PDC 模擬器) 設為權威的，讓其他網域控制站能返回正常運作狀態：  
>   
> [使用 BurFlags 登錄機碼重新初始化檔案複寫服務複本集](https://support.microsoft.com/kb/290762)  
>   
> [如何針對 DFSR 複寫的 SYSVOL (例如 FRS 的 "D4/D2") 強制執行權威及非權威的同步處理](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> 請勿在同一台 Hypervisor 主機上的樹系或網域中執行所有網域控制站。 這會在 Hypervisor 每次離線時導致單一失敗點，因為損壞 AD DS、Exchange、SQL 及其他企業作業。 這和整個網域或樹系只使用一個網域控制站的情況一樣。 在多個平台上有多個網域控制站有助於提供備援和容錯功能。  
  
#### <a name="post-snapshot-replication"></a>快照後複寫  
將建立快照後所有源自本機變更複寫輸出之前，請勿還原快照。 如果其他網域控制站尚未透過複寫接收變更，則所有原始變更都將永久遺失。  
  
使用 Repadmin.exe，顯示網域控制站及其協力電腦之間所有尚未複寫的輸出變更：  
  
1.  使用下列命令傳回 DC 的協力電腦名稱和 DSA 物件 GUID：  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  將協力電腦網域控制站擱置中的輸入複寫傳回要還原的網域控制站：  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
或者，只查看尚未複寫的變更次數：  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
例如 (輸出已加以修改方便閱讀，重要項目並***設為斜體***)，您可以在此處查看 DC4 的複寫夥伴關係：  
  
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
  
現在，您知道它正在使用 DC2 和 DC3 進行複寫。 接著，顯示 DC2 仍未從 DC4 取得變更的清單，並且出現一個新群組：  
  
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
  
您也可以測試其他協力電腦，以確定它尚未複寫。  
  
或者，如果您不在意哪些物件尚未複寫，只關心任何尚未處理的物件，則可使用 **/statistics** 選項：  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> 如果您看見任何失敗或尚未處理的複寫，請測試所有可寫入的協力電腦。 只要有一部電腦有交集，通常就能安全地還原快照，因為轉移複寫最終能夠與其他伺服器調解。  
>   
> 請務必注意複寫中透過 /showchanges 顯示的任何錯誤，在修正這些錯誤之前請勿繼續進行。  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Windows PowerShell 快照 Cmdlet  
下列 Windows PowerShell Hyper-V 模組 Cmdlet 可以提供 Windows Server 2012 中的快照功能：  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  
