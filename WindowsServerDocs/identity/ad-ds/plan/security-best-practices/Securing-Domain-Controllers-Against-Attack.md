---
description: 深入瞭解：保護網域控制站免于遭受攻擊
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: 保護網域控制站不受攻擊
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 06/18/2017
ms.topic: article
ms.openlocfilehash: 54540bd1c42f82c267e0986ba14ed09b2908f7e0
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040616"
---
# <a name="securing-domain-controllers-against-attack"></a>保護網域控制站不受攻擊

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

*定律3：如果不良的人對您的電腦有不受限制的實體存取，就不是您的電腦。* - [安全性 (2.0 版的十大不可變法則) ](https://www.microsoft.com/en-us/msrc?rtc=1)

網域控制站除了提供可讓企業有效管理伺服器、工作站、使用者和應用程式的服務和資料之外，還提供 AD DS 資料庫的實體儲存體。 如果有惡意使用者取得網域控制站的特殊存取權限，該使用者就可以修改、損毀或終結 AD DS 資料庫，以及由 Active Directory 管理的所有系統和帳戶。

因為網域控制站可以讀取和寫入 AD DS 資料庫中的任何資料，所以危及網域控制站的方式是，除非您能夠使用已知的良好備份進行復原，並關閉在進程中允許入侵的間隙，否則您的 Active Directory 樹系永遠都不會被視為值得信任。

根據攻擊者的準備、工具和技能、修改或甚至是無法修復的 AD DS 資料庫損毀，只要幾分鐘就能完成，而不是數天或數周。 重要的是，攻擊者對 Active Directory 有特殊的存取權，但在取得特殊許可權存取的時候，攻擊者已經規劃了多少時間。 危害網域控制站可以提供最有利的存取途徑，也就是成員伺服器、工作站和 Active Directory 的最直接途徑。 因此，網域控制站應該分開保護，比一般 Windows 基礎結構更 stringently。

## <a name="physical-security-for-domain-controllers"></a>網域控制站的實體安全性

本節提供實體保護網域控制站的相關資訊，無論網域控制站是實體或虛擬機器、資料中心位置、分公司，甚至只有基本基礎結構控制項的遠端位置。

### <a name="datacenter-domain-controllers"></a>資料中心網域控制站

#### <a name="physical-domain-controllers"></a>實體網域控制站

在資料中心中，實體網域控制站應該安裝在專用的安全機架或網架中，與一般伺服器人口不同。 可能的話，應該使用信賴平臺模組設定網域控制站 (TPM) 晶片，而域控制器伺服器中的所有磁片區都應該透過 BitLocker 磁碟機加密來保護。 BitLocker 通常會以單一位數的百分比增加效能負擔，但即使磁片已從伺服器移除，仍會保護目錄免于洩漏。 BitLocker 也可以協助保護系統免于遭受攻擊（例如 rootkit），因為修改開機檔會導致伺服器開機進入修復模式，如此就可以載入原始二進位檔。 如果網域控制站設定為使用軟體 RAID、序列連接 SCSI、SAN/NAS 存放裝置或動態磁碟區，則無法執行 BitLocker，因此在本機連接的存放裝置 (不論是否有硬體 RAID) 都應該在網域控制站中使用。

#### <a name="virtual-domain-controllers"></a>虛擬網域控制站

如果您執行虛擬網域控制站，您應該確定網域控制站是在與環境中其他虛擬機器不同的實體主機上執行。 即使您使用協力廠商虛擬化平臺，也請考慮在 Windows Server 2012 或 Windows Server 2008 R2 中的 Hyper-v 伺服器上部署虛擬網域控制站，以提供最少量的受攻擊面，並且可以使用它所裝載的網域控制站來管理，而不是使用其他虛擬化主機進行管理。 如果您執行的是 System Center Virtual Machine Manager (SCVMM) 來管理虛擬化基礎結構，您可以將系統管理工作委派給網域控制站虛擬機器所在的實體主機，以及將網域控制站本身授與授權的系統管理員。 您也應該考慮分隔虛擬網域控制站的儲存空間，以防止存放裝置系統管理員存取虛擬機器檔案。

### <a name="branch-locations"></a>分支位置

#### <a name="physical-domain-controllers-in-branches"></a>分支中的實體網域控制站

在多部伺服器的所在位置，但實際上並不會受到保護資料中心伺服器安全的程度，應使用 TPM 晶片設定實體網域控制站，並為所有伺服器磁片區設定 BitLocker 磁碟機加密。 如果網域控制站無法儲存在分支位置的鎖定空間中，您應該考慮在這些位置部署 Rodc。

#### <a name="virtual-domain-controllers-in-branches"></a>分支中的虛擬網域控制站

如果可能的話，您應該在分公司的不同實體主機上執行虛擬網域控制站，而不是在網站中的其他虛擬機器上執行。 在無法在不同的實體主機上執行虛擬網域控制站的分公司，您應該在虛擬網域控制站執行的虛擬網域控制站最少的主機上執行 TPM 晶片和 BitLocker 磁碟機加密，盡可能地執行所有主機。 根據分公司的大小與實體主機的安全性，您應該考慮在分支位置部署 Rodc。

### <a name="remote-locations-with-limited-space-and-security"></a>空間和安全性有限的遠端位置

如果您的基礎結構包含只能安裝單一實體伺服器的位置，則可以在遠端位置安裝能夠執行虛擬化工作負載的伺服器，而且應該將 BitLocker 磁碟機加密設定為保護伺服器中的所有磁片區。 伺服器上的一部虛擬機器應該執行 RODC，並將其他伺服器作為主機上的個別虛擬機器執行。 有關規劃 RODC 部署的詳細資訊，請參閱《 [唯讀網域控制站規劃和部署指南》](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771744(v=ws.10))。 如需有關部署和保護虛擬網域控制站的詳細資訊，請參閱 [在 hyper-v 中執行網域控制站](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd363553(v=ws.10))。 如需強化 Hyper-v、委派虛擬機器管理及保護虛擬機器的詳細指引，請參閱 Microsoft 網站上的 [Hyper-v 安全性指南](https://www.microsoft.com/download/details.aspx?id=16650) 解決方案加速器。

## <a name="domain-controller-operating-systems"></a>網域控制站作業系統

您應該在您組織內支援的最新 Windows Server 版本上執行所有網域控制站，並在網域控制站擴展中排定舊版作業系統的解除委任。 藉由讓網域控制站保持最新狀態並排除舊版網域控制站，您通常可以利用執行舊版作業系統的網域控制站可能無法在網域或樹系中使用的新功能和安全性。 網域控制站應該是全新安裝和升級，而不是從舊版作業系統或伺服器角色升級;也就是說，請勿就地升級網域控制站，或在未安裝作業系統的伺服器上執行 AD DS 安裝精靈。 藉由執行全新安裝的網域控制站，您可以確保舊版檔案和設定不會不慎留在網域控制站上，而且您可以簡化一致安全的網域控制站設定的強制執行。

## <a name="secure-configuration-of-domain-controllers"></a>保護網域控制站的設定

許多可自由使用的工具，其中有些預設會安裝在 Windows 中，可用來為網域控制站建立初始安全性設定基準，之後可由 Gpo 強制執行。 這些工具會在此說明。

### <a name="security-configuration-wizard"></a>安全性設定精靈

所有網域控制站都應該在初始組建時鎖定。 您可以使用 Windows Server 原生髮行的「安全性設定向導」來完成這項操作，以在「基底組建」網域控制站上設定服務、登錄、系統和 WFAS 設定。 設定可以儲存並匯出至 GPO，以連結至樹系中每個網域中的網域控制站 OU，以強制執行一致的網域控制站設定。 如果您的網域包含多個版本的 Windows 作業系統，您可以設定 Windows Management Instrumentation (WMI) 篩選器，只將 Gpo 套用到執行對應作業系統版本的網域控制站。

### <a name="microsoft-security-compliance-toolkit"></a>Microsoft 安全性合規性工具組

[Microsoft 安全性合規性工具](https://microsoft.com/download/details.aspx?id=55319) 組網域控制站設定可以與安全性設定向導設定結合，以針對部署于 Active Directory 中網域控制站 OU 上的 gpo 部署和強制執行的網域控制站產生全方位的設定基準。

### <a name="rdp-restrictions"></a>RDP 限制

連結至樹系中所有網域控制站 Ou 的群組原則物件，應設定為只允許來自授權的使用者和系統的 RDP 連線 (例如，[跳過伺服器]) 。 這可以透過使用者權限設定和 WFAS 設定的組合來達成，而且應該在 Gpo 中執行，以便一致地套用原則。 如果略過，下一次群組原則重新整理會將系統傳回其適當的設定。

### <a name="patch-and-configuration-management-for-domain-controllers"></a>網域控制站的修補和設定管理

雖然看起來可能會違反直覺，但您應該考慮將網域控制站和其他重要的基礎結構元件與一般 Windows 基礎結構分開。 如果您的基礎結構中的所有電腦都利用企業設定管理軟體，則危害系統管理軟體可以用來危害或終結該軟體管理的所有基礎結構元件。 藉由將網域控制站的修補和系統管理與一般人口區分開，您就可以減少安裝在網域控制站上的軟體數量，以及嚴密控制其管理。

### <a name="blocking-internet-access-for-domain-controllers"></a>封鎖網域控制站的網際網路存取

在 Active Directory 安全性評定中執行的其中一項檢查，就是在網域控制站上使用和設定 Internet Explorer。 Internet Explorer (或其他任何網頁瀏覽器) 都不能在網域控制站上使用，但是對數以千計的網域控制站進行分析，有許多情況下，有特殊許可權的使用者可以使用 Internet Explorer 流覽組織的內部網路或網際網路。

如先前的「設定錯誤」一節中所 [述，使用](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)高度特殊的帳戶流覽網際網路 (或受感染的內部網路) 從 Windows 基礎結構中最強大的一部電腦， (這是唯一允許在本機登入網域控制站的帳戶，) 對組織的安全性有相當的風險。 無論是透過下載或下載受惡意程式碼感染的「公用程式」，攻擊者都能存取完全入侵或終結 Active Directory 環境所需的一切。

雖然 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008 和目前版本的 Internet Explorer 為惡意下載提供一些保護，但在大部分情況下，使用網域控制站和特殊許可權帳戶來流覽網際網路、網域控制站執行 Windows Server 2003，或由較新的作業系統和瀏覽器所提供的保護已刻意停用。

在網域控制站上啟動網頁瀏覽器時，不應該只由原則禁止，而是必須由技術控制項和網域控制站來存取網際網路。 如果您的網域控制站需要跨網站複寫，您應該在網站之間執行安全的連線。 雖然詳細的設定指示不在本檔的討論範圍內，但您可以執行一些控制項，以限制網域控制站被誤用或設定不正確的能力，以及後續遭到入侵的能力。

### <a name="perimeter-firewall-restrictions"></a>周邊防火牆限制

應設定周邊防火牆，以封鎖從網域控制站到網際網路的輸出連線。 雖然網域控制站可能需要跨網站界限進行通訊，但可以將周邊防火牆設定為允許網站間通訊，方法是遵循如何在 Microsoft 支援服務網站上 [設定 Active Directory 網域和信任的防火牆](https://support.microsoft.com/kb/179442) 中提供的指導方針。

### <a name="dc-firewall-configurations"></a>DC 防火牆設定

如先前所述，您應該使用 [安全性設定] 嚮導來捕捉網域控制站上 [具有 Advanced 安全性的 Windows 防火牆] 的設定。 您應該檢查安全性設定 Wizard 的輸出，以確保防火牆設定符合您組織的需求，然後使用 Gpo 來強制執行設定。

### <a name="preventing-web-browsing-from-domain-controllers"></a>防止從網域控制站進行網頁流覽

您可以使用 AppLocker 設定、「黑洞」 proxy 設定和 WFAS 設定的組合，以防止網域控制站存取網際網路，以及防止在網域控制站上使用網頁瀏覽器。
