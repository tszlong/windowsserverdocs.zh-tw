---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: 保護網域控制站不受攻擊
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6be2899e85b68578518d9de1805c287367608163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863089"
---
# <a name="securing-domain-controllers-against-attack"></a>保護網域控制站不受攻擊

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

*法律數字三：如果不肖份子具有不受限制實體存取您的電腦，就您的電腦淪陷了。* - [十大不變法則安全性 （2.0 版）](https://technet.microsoft.com/security/hh278941.aspx)  
  
網域控制站提供 AD DS 資料庫，除了提供服務和資料可讓企業更有效管理其伺服器、 工作站、 使用者和應用程式的實體儲存體。 該使用者遭到惡意使用者取得特殊權限的存取權的網域控制站時，如果可以修改、 損毀，或終結 AD DS 資料庫和延伸模組的所有系統和 Active Directory 所管理的帳戶。  
  
遭到入侵，網域控制站因為網域控制站可以讀取和寫入 AD DS 資料庫中的任何項目，表示，您的 Active Directory 樹系可以永遠不會再考慮使用 trustworthy 除非您可以使用已知的良好備份進行復原，關閉處理序中允許遭到入侵的漏洞。  
  
攻擊者的準備、 工具和技巧，修改或甚至是無法修復的損毀到 AD DS 而定，資料庫可以小時，而不天或數週以分鐘為單位來完成。 重要的不多久要攻擊者有特殊權限存取 Active Directory，但多少的目前權限，攻擊者有計劃時取得。 影響的網域控制站，可以提供最有利路徑存取的大規模傳播或解構的成員伺服器、 工作站和 Active Directory 的最直接路徑。 基於這個原因，個別且比一般的 Windows 基礎結構更嚴格，應該進行保護的網域控制站。  

## <a name="physical-security-for-domain-controllers"></a>網域控制站的實體安全性

本節中的實際保護網域控制站、 網域控制站是否為實體或虛擬機器，在資料中心位置，分公司中，只有基本基礎結構的控制項甚至遠端位置的相關資訊。  
  
### <a name="datacenter-domain-controllers"></a>資料中心的網域控制站  
  
#### <a name="physical-domain-controllers"></a>實體的網域控制站

在資料中心，實體的網域控制站應該安裝在專用的安全機架或機殼，有別於一般伺服器母體擴展。 可能的話，請使用信賴平台模組 (TPM) 晶片，應該設定網域控制站和網域控制站伺服器中的所有磁碟區應受到保護，透過 BitLocker 磁碟機加密。 BitLocker 通常會以單一數字百分比會增加效能負擔，但保護免於遭受洩露的目錄，即使磁碟會從伺服器移除。 BitLocker 也有助於保護系統以防範攻擊，例如 rootkit，因為開機檔案的修改會導致伺服器開機進入修復模式，以便可以載入原始的二進位檔。 如果網域控制站設定為使用軟體 RAID、 序列連結 SCSI、 SAN/NAS 存放裝置，或無法實作動態磁碟區，BitLocker，因此本機連接的存放裝置 （不論有無硬體 RAID） 應該用於網域控制站時可能的。  
  
#### <a name="virtual-domain-controllers"></a>虛擬網域控制站 

如果您實作虛擬網域控制站時，您應該確定在個別的實體主機，比其他環境中的虛擬機器上執行網域控制站。 即使您使用協力廠商虛擬化平台，請考慮部署在 Windows Server 2012 或 Windows Server 2008 R2，以提供最少的受攻擊面，並可透過它所主控的網域控制站管理的 HYPER-V 伺服器上的虛擬網域控制站而非管理的虛擬化主機的其餘部分。 如果您實作 System Center Virtual Machine Manager (SCVMM) 管理您的虛擬化基礎結構時，您可以將委派管理的網域控制站虛擬機器位於和網域控制站上的實體主機本身已獲授權的系統管理員。 您也應該考慮將虛擬網域控制站，以防止存放裝置系統管理員存取的虛擬機器檔案的儲存體。  
  
### <a name="branch-locations"></a>分公司  
  
#### <a name="physical-domain-controllers-in-branches"></a>在分支中的實體網域控制站

在多部伺服器位於但不是在受保護資料中心伺服器的程度來實際安全的位置，實體的網域控制站應該設定 TPM 晶片與 BitLocker 磁碟機加密的所有伺服器磁碟區。 如果網域控制站無法儲存在分公司地點上鎖的機房內，您應該考慮部署 Rodc，在這些位置。  
  
#### <a name="virtual-domain-controllers-in-branches"></a>在分支中的虛擬網域控制站

可能的話，您應該在分公司執行虛擬網域控制站，在個別的實體主機，比其他虛擬機器，在站台上項目。 在分公司所在虛擬網域控制站無法在虛擬伺服器母體擴展的其餘部分不同的實體主機執行，您應該實作 TPM 晶片和 BitLocker 磁碟機加密虛擬網域控制站執行至少所在的主機上和如果可能的話所有主機。 根據分公司的大小和實體主機的安全性，您應該考慮部署 Rodc 分公司地點。  
  
### <a name="remote-locations-with-limited-space-and-security"></a>具有有限的空間和安全性的遠端位置

如果您的基礎結構會包含只有一部實體伺服器可以安裝所在的位置，能夠執行虛擬化工作負載的伺服器應該安裝在遠端位置，並且應該設定 BitLocker 磁碟機加密，保護所有在伺服器中的磁碟區。 一部伺服器上的虛擬機器應該執行的 RODC，與其他主機上當做個別的虛擬機器執行的伺服器。 資訊中提供的 RODC 部署規劃[唯讀網域控制站的規劃與部署指南 》](https://go.microsoft.com/fwlink/?LinkID=135993)。 如需有關部署和保護虛擬的網域控制站的詳細資訊，請參閱 <<c0> [ 在 HYPER-V 執行網域控制站](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx)TechNet 網站上。 如需詳細指引，來強化 HYPER-V 時，委派給虛擬機器管理及保護虛擬機器，請參閱[HYPER-V 安全性指南](https://www.microsoft.com/download/details.aspx?id=16650)Microsoft 網站上的解決方案加速器。  
  
## <a name="domain-controller-operating-systems"></a>網域控制站作業系統

您應該在最新版本的支援組織內，並優先處理解除委任網域控制站母體擴展中的舊版作業系統的 Windows server 上執行所有網域控制站。 藉由保留目前和消除舊版網域控制站的網域控制站，您通常可能需要利用新功能，以及可能無法在網域或樹系與網域控制站執行舊版作業系統的安全性。 網域控制站應該是全新安裝和升級，而非升級從舊版作業系統或伺服器角色;也就是說，不要執行就地升級的網域控制站或最近未安裝作業系統所在的伺服器上執行 AD DS 安裝精靈。 藉由實作全新安裝的網域控制站，您可以確保舊版檔案和設定會不小心保留網域控制站上，並簡化一致且安全的網域控制站設定強制執行。  
  
## <a name="secure-configuration-of-domain-controllers"></a>安全的網域控制站設定

一些免費工具，其中有些會安裝在 Windows 中的預設值，可用來建立初始安全性組態基準的後續 Gpo 來強制執行的網域控制站。 這些工具是此處所述。  
  
### <a name="security-configuration-wizard"></a>安全性設定精靈  

所有網域控制站應該都鎖定初始建置時。 這可以使用安全性設定精靈中設定服務、 登錄、 系統和 WFAS 設定 「 基底的組建 」 網域控制站上的 Windows Server 隨附原生方式來達成。 設定可儲存並匯出至可連結到樹系中每個網域中強制執行一致的組態網域控制站的網域控制站 OU 的 GPO。 如果您的網域包含多個版本的 Windows 作業系統，您可以設定 Gpo 只套用至網域控制站執行對應的作業系統版本的 Windows Management Instrumentation (WMI) 篩選器。  
  
### <a name="microsoft-security-compliance-toolkit"></a>Microsoft 安全性合規性工具組

[Microsoft 安全性合規性工具組](https://www.microsoft.com/download/details.aspx?id=55319)網域控制站設定可以結合以產生完整的組態基準部署和強制執行 Gpo 的網域控制站的安全性設定精靈設定部署在 Active Directory 中網域控制站 OU。  
  
### <a name="rdp-restrictions"></a>RDP 的限制

連結至所有網域控制站的樹系中的 Ou 的群組原則物件應該設定為允許 RDP 連線只能從授權的使用者和系統 （例如，「 跳躍伺服器 」）。 這可藉由結合使用者權限設定以及 WFAS 組態，並應該在 Gpo 中實作，以便一致地套用原則。 如果會略過，下次重新整理群組原則會回到其適當的組態系統。  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>修補程式和網域控制站的組態管理

雖然似乎自相矛盾，但是您應該考慮修補網域控制站和其他重要的基礎結構元件與一般的 Windows 基礎結構分開。 如果您利用基礎結構中的所有電腦的企業設定管理軟體，這些系統管理軟體的危害可用來危害或摧毀該軟體所管理的所有基礎結構元件。 藉由分隔從一般的母體擴展的網域控制站的修補程式和系統管理，您可以減少網域控制站，除了之外緊密控制管理其上安裝的軟體。
  
### <a name="blocking-internet-access-for-domain-controllers"></a>封鎖網際網路存取的網域控制站  

其中一個 Active Directory 安全性評定的過程中執行的檢查是使用和網域控制站上的 Internet Explorer 設定。 Internet Explorer （或任何其他網頁瀏覽器） 不應在網域控制站，但分析的數千個網域控制站已顯示許多情況下，在其中具有特殊權限的使用者，請用來瀏覽組織的內部網路的 Internet Explorer 或網際網路。  
  
如先前所述的 「 設定不正確 」 一節[入侵途徑](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)，其中一個最強大的電腦，使用具有高度權限的 Windows 基礎結構中，瀏覽網際網路 （或受感染的內部網路）（此為僅允許本機登入網域控制站預設的帳戶） 的帳戶提供異常組織的安全性風險。 不論是透過下載或下載惡意程式碼感染 「 公用程式 」 的磁碟機，攻擊者可以存取他們需要完全危害或摧毀 Active Directory 環境的所有項目。  
  
雖然 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008 和目前版本的 Internet Explorer 提供許多的保護以防範惡意下載，在大部分情況下，在網域控制站和特殊權限的帳戶已使用以瀏覽網際網路、 網域控制站執行 Windows Server 2003 或更新版本的作業系統和瀏覽器所提供的保護已刻意停用。  
  
應該禁止啟動網域控制站上的網頁瀏覽器，不只依原則，但技術控制項，且網域控制站不應該允許存取網際網路。 如果您的網域控制站需要站台間複寫，您應該實作在站台之間的安全連線。 雖然詳細的設定指示超出本文件的範圍，您可以實作多個控制項來限制誤用或設定不正確並接著洩露網域控制站的能力。  
  
### <a name="perimeter-firewall-restrictions"></a>周邊防火牆限制

周邊防火牆應該設定為封鎖傳出連線到網際網路的網域控制站。 雖然跨站台界限進行通訊時，可能需要網域控制站，可以設定周邊防火牆以允許站台間的通訊中所提供的指導方針[如何設定防火牆供網域及信任](https://support.microsoft.com/kb/179442) Microsoft 支援網站上。  
  
### <a name="dc-firewall-configurations"></a>DC 防火牆組態  

如先前所述，您應該使用安全性設定精靈來擷取網域控制站上具有進階安全性的 Windows 防火牆的組態設定。 您應該檢閱以確定防火牆組態設定符合貴組織的需求，然後使用 強制執行組態設定的 Gpo 的安全性設定精靈 的輸出。  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>防止從網域控制站的網頁瀏覽

若要存取網際網路時，防止網域控制站，並防止網域控制站上的網頁瀏覽器使用，您可以使用 AppLocker 組態、 「 黑洞 」 proxy 設定和 WFAS 組態的組合。
