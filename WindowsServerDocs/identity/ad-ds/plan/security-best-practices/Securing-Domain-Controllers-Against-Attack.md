---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: 保護網域控制站不受攻擊
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b91164e14f3ae94f6f7d01c62125f45124df0907
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367617"
---
# <a name="securing-domain-controllers-against-attack"></a>保護網域控制站不受攻擊

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

@no__t 0Law 數位3：如果不良的人員對您的電腦擁有不受限制的實體存取權，就不是您的電腦了。 * - [十個不變的安全性法則（版本2.0）](https://technet.microsoft.com/security/hh278941.aspx)  
  
除了提供可讓企業有效管理其伺服器、工作站、使用者和應用程式的服務和資料以外，網域控制站還提供 AD DS 資料庫的實體儲存體。 如果惡意使用者取得網域控制站的特殊許可權存取權，該使用者就可以修改、損毀或終結 AD DS 資料庫，並根據延伸模組，Active Directory 所管理的所有系統和帳戶。  
  
因為網域控制站可以讀取和寫入 AD DS 資料庫中的任何專案，所以網域控制站的危害意味著您的 Active Directory 樹系永遠不會被視為值得信任，除非您可以使用已知的正確備份來復原，以及關閉允許在進程中洩露的間隙。  
  
視攻擊者的準備、工具和技能而定，在數分鐘到數小時內（而非數天或數周）可以完成修改或甚至是無法修復的 AD DS 資料庫損毀。 重要的是，攻擊者具有 Active Directory 的特殊許可權存取權，但在取得特殊許可權存取時，攻擊者已計畫多少時間。 危害網域控制站可提供最有利的存取權，以進行大規模的存取傳播，或最直接的成員伺服器、工作站和 Active Directory 路徑。 因此，網域控制站應分別受到保護，而且比一般的 Windows 基礎結構更而更加嚴格。  

## <a name="physical-security-for-domain-controllers"></a>網域控制站的實體安全性

本節提供實體保護網域控制站的相關資訊，不論網域控制站是實體或虛擬機器、在資料中心位置、分公司，甚至是只有基本基礎結構控制項的遠端位置。  
  
### <a name="datacenter-domain-controllers"></a>Datacenter 網域控制站  
  
#### <a name="physical-domain-controllers"></a>實體網域控制站

在資料中心中，實體網域控制站應該安裝在與一般伺服器擴展不同的專用安全機架或機殼中。 可能的話，應該使用信賴平臺模組（TPM）晶片來設定網域控制站，而域控制器伺服器中的所有磁片區都應該透過 BitLocker 磁碟機加密來保護。 BitLocker 通常會在單一位數的百分比中增加效能額外負荷，但即使已從伺服器移除磁片，還是會保護目錄免于危害。 BitLocker 也可以協助保護系統免于遭受攻擊，例如 rootkit，因為修改開機檔會導致伺服器開機進入修復模式，以便載入原始的二進位檔。 如果網域控制站設定為使用軟體 RAID、序列連接 SCSI、SAN/NAS 存放裝置或動態磁碟區，則無法執行 BitLocker，因此每次都應該在網域控制站中使用本機連接的存放裝置（不論是否使用硬體 RAID）的話.  
  
#### <a name="virtual-domain-controllers"></a>虛擬網域控制站 

如果您執行虛擬網域控制站，您應該確保網域控制站在不同的實體主機上執行，而不是環境中的其他虛擬機器。 即使您使用協力廠商的虛擬化平臺，還是可以考慮在 Windows Server 2012 或 Windows Server 2008 R2 中的 Hyper-v 伺服器上部署虛擬網域控制站，這會提供最小的受攻擊面，並可透過其主控的網域控制站來管理而不是使用其他虛擬化主機來管理。 如果您執行 System Center Virtual Machine Manager （SCVMM）來管理虛擬化基礎結構，您可以針對網域控制站虛擬機器所在的實體主機和網域控制站，委派系統管理。本身授與授權的系統管理員。 您也應該考慮分隔虛擬網域控制站的存放裝置，以防止存放裝置系統管理員存取虛擬機器檔案。  
  
### <a name="branch-locations"></a>分支位置  
  
#### <a name="physical-domain-controllers-in-branches"></a>分支中的實體網域控制站

在多部伺服器所在的位置，但實際上並未受到保護，以確保資料中心伺服器的安全，應針對所有伺服器磁片區使用 TPM 晶片和 BitLocker 磁碟機加密來設定實體網域控制站。 如果網域控制站不能儲存在分公司位置的鎖定聊天室中，您應該考慮在這些位置部署 Rodc。  
  
#### <a name="virtual-domain-controllers-in-branches"></a>分支中的虛擬網域控制站

可能的話，您應該在分公司的不同實體主機上執行虛擬網域控制站，而不是網站中的其他虛擬機器。 在虛擬網域控制站不能在其他虛擬伺服器擴展的不同實體主機上執行的分公司，您應該在最少執行虛擬網域控制站的主機上，執行 TPM 晶片和 BitLocker 磁碟機加密，所有主機（如果可能）。 視分公司的大小和實體主機的安全性而定，您應該考慮在分支位置部署 Rodc。  
  
### <a name="remote-locations-with-limited-space-and-security"></a>空間和安全性有限的遠端位置

如果您的基礎結構包含的位置只能安裝單一實體伺服器，則能夠執行虛擬化工作負載的伺服器應該安裝在遠端位置，而且應該設定 BitLocker 磁碟機加密以保護所有伺服器中的磁片區。 伺服器上的一部虛擬機器應該執行 RODC，而其他伺服器則以主機上的個別虛擬機器的形式執行。 《[唯讀網域控制站規劃與部署指南》](https://go.microsoft.com/fwlink/?LinkID=135993)中提供規劃 RODC 部署的相關資訊。 如需部署和保護虛擬網域控制站的詳細資訊，請參閱 TechNet 網站上的[在 hyper-v 中執行網域控制站](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx)。 如需強化 Hyper-v、委派虛擬機器管理及保護虛擬機器的詳細指引，請參閱 Microsoft 網站上的[Hyper-v 安全性指南](https://www.microsoft.com/download/details.aspx?id=16650)解決方案加速器。  
  
## <a name="domain-controller-operating-systems"></a>網域控制站作業系統

您應該在組織內支援的最新 Windows Server 版本上執行所有網域控制站，並優先處理網域控制站擴展中舊版作業系統的解除委任。 藉由讓網域控制站維持在最新狀態並排除舊版網域控制站，您通常可以利用在執行舊版作業系統的網域控制站的網域或樹系中可能無法使用的新功能和安全性。 應該以全新的安裝和升級網域控制站，而不是從舊版作業系統或伺服器角色升級;也就是說，請勿執行網域控制站的就地升級，或是在未安裝作業系統的伺服器上執行 AD DS 安裝精靈。 藉由執行全新安裝的網域控制站，您可以確保舊版檔案和設定不會不慎留在網域控制站上，並簡化一致且安全的網域控制站設定強制執行。  
  
## <a name="secure-configuration-of-domain-controllers"></a>保護網域控制站的設定

有些免費的工具，其中有些是預設會安裝在 Windows 中，可用來建立網域控制站的初始安全性設定基準，之後可由 Gpo 強制執行。 這裡有這些工具的說明。  
  
### <a name="security-configuration-wizard"></a>安全性設定精靈  

所有網域控制站都應該在初始組建時鎖定。 您可以使用 Windows Server 原生提供的安全性設定向導，在「基底組建」網域控制站上設定服務、登錄、系統和 WFAS 設定，以達成此目的。 設定可以儲存並匯出至 GPO，以連結至樹系中每個網域的網域控制站 OU，以強制執行一致的網域控制站設定。 如果您的網域包含多個版本的 Windows 作業系統，您可以設定 Windows Management Instrumentation （WMI）篩選器，只將 Gpo 套用至執行對應作業系統版本的網域控制站。  
  
### <a name="microsoft-security-compliance-toolkit"></a>Microsoft 安全性合規性工具組

[Microsoft 安全性合規性工具](https://www.microsoft.com/download/details.aspx?id=55319)組 [網域控制站設定] 可以與 [安全性設定] [Wizard] 設定結合，針對部署于的 gpo 部署和強制執行的網域控制站產生完整設定基準。Active Directory 中的網域控制站 OU。  
  
### <a name="rdp-restrictions"></a>RDP 限制

連結至樹系中所有網域控制站 Ou 的群組原則物件，應設定為僅允許來自授權使用者和系統的 RDP 連線（例如，跳躍伺服器）。 這可以透過使用者權限設定和 WFAS 設定的組合來達成，而且應該在 Gpo 中執行，以便一致地套用原則。 如果略過，下一個群組原則重新整理會將系統傳回其適當的設定。  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>網域控制站的修補和設定管理

雖然看起來似乎違反直覺，但您應該考慮將網域控制站和其他重要的基礎結構元件與一般的 Windows 基礎結構分開修補。 如果您的基礎結構中的所有電腦都使用企業設定管理軟體，系統管理軟體的危害可用來危害或摧毀該軟體管理的所有基礎結構元件。 藉由將網域控制站的修補和系統管理與一般擴展分開，您可以減少安裝在網域控制站上的軟體數量，以及嚴格控制其管理。
  
### <a name="blocking-internet-access-for-domain-controllers"></a>封鎖網域控制站的網際網路存取  

在 Active Directory 安全性評估中執行的其中一項檢查，就是在網域控制站上使用和設定 Internet Explorer。 Internet Explorer （或任何其他網頁瀏覽器）不應在網域控制站上使用，但對上千個網域控制站的分析，發現有許多情況下，有許可權的使用者使用 Internet Explorer 流覽組織的內部網路或網際.  
  
如先前在「設定錯誤」一節中所述，使用高許可權帳戶從 Windows 基礎結構[中的其中](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)一個最強大的電腦流覽網際網路（或受感染的內部網路）（這是唯一的預設允許登入網域控制站的帳戶，會對組織的安全性帶來極危險的風險。 無論是透過下載或下載惡意程式碼感染的「公用程式」，攻擊者都可以存取所需的所有專案，以完全洩露或摧毀 Active Directory 環境。  
  
雖然 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008 和目前版本的 Internet Explorer 提供一些保護以防止惡意下載，但在大部分情況下，已使用網域控制站和特殊許可權帳戶來流覽網際網路、網域控制站執行的是 Windows Server 2003，或已刻意停用較新作業系統和瀏覽器所提供的保護。  
  
在網域控制站上啟動網頁瀏覽器時，應該不只能由原則使用，而是由技術控制項禁止，且網域控制站不得存取網際網路。 如果您的網域控制站需要跨網站複寫，您應該在網站之間執行安全的連線。 雖然詳細的設定指示不在本檔的討論範圍內，但您可以執行一些控制項來限制網域控制站誤用或設定錯誤的能力，以及後續的入侵。  
  
### <a name="perimeter-firewall-restrictions"></a>周邊防火牆限制

周邊防火牆應設定為封鎖從網域控制站到網際網路的輸出連線。 雖然網域控制站可能需要跨網站界限進行通訊，但您可以遵循[如何設定網域和信任的防火牆](https://support.microsoft.com/kb/179442)中所提供的指導方針，將周邊防火牆設定為允許站對站通訊Microsoft 支援服務網站。  
  
### <a name="dc-firewall-configurations"></a>DC 防火牆設定  

如先前所述，您應該使用 [安全性設定] 嚮導來捕獲網域控制站上 [具有 Advanced Security 的 Windows 防火牆] 的設定。 您應該檢查安全性設定向導的輸出，以確保防火牆設定會符合您組織的需求，然後使用 Gpo 來強制執行設定。  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>防止從網域控制站進行網頁流覽

您可以使用 AppLocker 設定、「黑洞」 proxy 設定和 WFAS 設定的組合，以防止網域控制站存取網際網路，以及防止在網域控制站上使用網頁瀏覽器。
