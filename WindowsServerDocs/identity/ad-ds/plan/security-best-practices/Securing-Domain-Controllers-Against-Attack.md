---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: "保護網域控制站攻擊"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc40d0ac536b128b799006214e360fa4d991ee44
ms.sourcegitcommit: 06a84f5caeab49b8480d6eed037aebbb59c77a9f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2018
---
# <a name="securing-domain-controllers-against-attack"></a>保護網域控制站攻擊

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

*法律數字三： 如果份子有無限制的實體存取您的電腦，就不您的電腦是。* - [安全性 （2.0 版） 的 10 定律](https://technet.microsoft.com/security/hh278941.aspx)  
  
網域控制站 AD DS 資料庫，除了提供的服務和資料，可讓企業有效管理的伺服器、 工作站、 使用者和應用程式提供實體的儲存空間。 使用者惡意使用者取得網域控制站存取權限時，是否可以修改、 損壞，或破壞 AD DS 資料庫和延伸的所有受 Active Directory 帳號和系統。  
  
網域控制站可讀取和寫入到 AD DS 資料庫中的任何項目，因為危害的網域控制站表示 Active Directory 樹系可以永遠不會被視為高可信度再試一次除非您無法復原使用已知的良好備份，並關閉 [允許此程序危害縫隙。  
  
攻擊者甚至關閉損壞到 AD DS 或修改準備、 工具，以及技能，而定，資料庫可以的時間，而不天或星期分鐘來完成。 事情的無法攻擊有特殊權限存取 Active Directory，但幅度攻擊者已經計劃的存取權限時，所以取得方式時間。 危害網域控制站可以提供最有利路徑廣泛地傳播的存取或直接最破壞成員伺服器、 工作站和 Active Directory 路徑。 因此，應該分開和比一般的 Windows 基礎結構嚴格保護網域控制站。  

  
## <a name="physical-security-for-domain-controllers"></a>實體網域控制站安全性  
本節實際保護網域控制站、 實體網域控制站是否或虛擬電腦，datacenter 位置、 分公司，甚至遠端位置與只有基礎的控制項中的相關資訊。  
  
### <a name="datacenter-domain-controllers"></a>Datacenter 網域控制站  
  
#### <a name="physical-domain-controllers"></a>實體網域控制站  
能源、 實體網域控制站專用安全架或與一般伺服器擴展不同的欄中安裝。 可能的話，應該設定的網域控制站晶片信賴平台模組 (TPM)，並網域控制站伺服器中的所有磁碟區應該受透過 BitLocker 磁碟機加密。 BitLocker 通常新增效能費用位數百分比，但保護中毒 directory 即使從伺服器移除的磁碟。 BitLocker 也可協助保護系統，例如 rookit 攻擊因為修改開機檔案將會導致開機進入修復模式，可載入原始二進位檔案伺服器。 如果網域控制站設定為使用軟體 RAID 序號連接 SCSI，舊日 NAS 存放裝置，或無法實作動態磁碟區，BitLocker，應網域控制站儘使用到本機連接的儲存 （無論硬體 RAID）。  
  
#### <a name="virtual-domain-controllers"></a>Virtual 網域控制站  
如果實作 virtual 網域控制站，您應該確定網域控制站的環境中其他虛擬高於不同實體主機上執行。 即使您使用協力廠商模擬平台，請考慮部署 virtual 的網域控制站在 Windows Server 2012 或 Windows Server 2008 R2，提供最少攻擊 surface HYPER-V Server 和可以來管理它裝載而非使用中的其餘部分模擬主機受管理的網域控制站。 如果您對模擬基礎結構管理實作系統中心一樣管理員 (SCVMM)，您可以實體主機上的網域控制站虛擬的電腦位於網域控制站本身授權的系統管理員的管理委派。 您也應該分隔 virtual 網域控制站，以避免存放裝置的系統管理員存取一樣檔案的存放裝置。  
  
### <a name="branch-locations"></a>分支 」 的位置  
  
#### <a name="physical-domain-controllers"></a>實體網域控制站  
在多部伺服器位於但實際並不安全的受保護資料中心伺服器程度實體網域控制站應該設定 BitLocker 磁碟機加密 TPM 晶片與的所有伺服器磁碟區。 如果您無法儲存網域控制站鎖定分支 」 的位置在俱樂部中，您應該部署 Rodc 那些位置。  
  
#### <a name="virtual-domain-controllers"></a>Virtual 網域控制站  
可能的話，您應該會在分公司執行 virtual 網域控制站在網站中的其他虛擬機器比不同實體主機上。 在分公司 virtual 網域控制站無法在其中執行中的其餘部分 isp 擴展從不同的實體主機，您應該在主機 virtual 網域控制站執行最小，並盡可能所有主機上實作 TPM 晶片和 BitLocker 磁碟機加密。 根據您的分公司大小和實體主機的安全性，您應該部署 Rodc 分支 」 的位置中。  
  
### <a name="remote-locations-with-limited-space-and-security"></a>安全性與的有限的空間遠端位置  
如果您的基礎結構包含的位置中，只有單一實體伺服器可以安裝，應該可以執行的工作負載模擬伺服器安裝遠端的位置，並設定 BitLocker 磁碟機加密保護所有伺服器磁碟區。 在伺服器上一個一樣應該先執行 RODC，與其他主機上的身分獨立虛擬電腦的伺服器。 規劃 RODC 部署中所提供的資訊[唯讀網域控制站規劃和部署指南](https://go.microsoft.com/fwlink/?LinkID=135993)。 如需部署及保護模擬的網域控制站的相關資訊，請查看[HYPER-V 中的網域控制站執行](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx)TechNet 網站上。 委派一樣管理及保護虛擬電腦，如需詳細指導方針強化 HYPER-V，查看[HYPER-V 安全性指南](https://www.microsoft.com/download/details.aspx?id=16650)Microsoft 網站上方案快速鍵。  
  
## <a name="domain-controller-operating-systems"></a>網域控制站作業系統  
您應該執行所有網域控制站在最新版本的 Windows Server 在組織中支援並解除委任的網域控制站擴展中的舊版作業系統的優先順序。 保留您的網域控制站的目前與消除舊版網域控制站，您通常可以拍攝的新功能，以及安全性可能無法使用網域或森林中的網域控制站執行舊版作業系統的優點。 網域控制站應該剛安裝和升級而不是從先前的作業系統或伺服器角色; 升級也就是請勿執行網域控制站的位置在升級或執行 AD DS 安裝精靈中的作業系統未剛安裝的伺服器上。 藉由實作剛安裝的網域控制站，就可以確保舊版檔案與設定的不小心左網域控制站上和您簡化執法一致且安全的網域控制站設定。  
  
## <a name="secure-configuration-of-domain-controllers"></a>網域控制站的安全設定  
許多免費的工具，其中一些安裝在 Windows 中的預設，可用於建立網域控制站的後續由 Gpo 初始的安全性設定基準。 這些工具如下所示。  
  
### <a name="security-configuration-wizard"></a>安全性設定精靈  

所有網域控制站應該都鎖定時初始建置。 這可以使用推出原生設定服務、 登錄、 系統，以及 WFAS 設定 「 基礎建置 」 的網域控制站的 Windows Server 中的安全性設定精靈達成。 儲存，匯出至 GPO 可以連結到網域控制站組織單位森林中的每個網域中執行設定的網域控制站的一致性的設定。 如果您的網域包含多個版本的 Windows 作業系統，您可以設定 Windows 管理檢測 (WMI) 篩選器 Gpo 僅適用於執行的作業系統版本相對應的網域控制站。  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
[Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx)安全性設定精靈設定製作網域控制站部署及 Gpo 部署網域控制站在 Active Directory 組織單位，來執行完整的設定基準可以結合網域控制站設定。  
  
### <a name="applocker"></a>AppLocker  
AppLocker 或第三方應用程式家工具應服務和應用程式，允許執行網域控制站上設定，這些允許的應用程式與服務應該組成只是必要的主機 AD DS，可能是 DNS，加上所有系統安全性軟體，例如防毒軟體的電腦。 根據家網域控制站允許的應用程式，這樣即使上網域控制站安裝未經授權的應用程式，則無法執行應用程式加入一層額外的安全性。  
  
### <a name="rdp-restrictions"></a>RDP 限制  
連結到 [所有網域控制站 Ou 森林中的群組原則物件應會設定為 RDP 連接只允許授權的使用者及系統（例如，捷徑伺服器）。 這可透過 WFAS 設定使用者權限設定的組合達到，應該實施 Gpo 使一致套用原則。 如果會略過、 下一步的群組原則重新整理會系統的正確設定。  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>更新和網域控制站的組態管理  
雖然它看起來直覺式，您應該修補網域控制站和其他重要的基礎結構元件分開您一般的 Windows 基礎結構。 如果您在您的基礎結構利用所有電腦的企業設定管理軟體，系統管理軟體的危害可用來危害或破壞所有的基礎結構元件由該軟體。 從一般的擴展分隔網域控制站的更新及系統管理，您可以減少的網域控制站除了密切控制他們管理上安裝軟體。  
  
### <a name="blocking-internet-access-for-domain-controllers"></a>網域控制站封鎖網際網路存取權  

檢查在 Active Directory 安全性評估執行的是使用與網域控制站的 Internet Explorer 的設定。 Internet Explorer （或任何其他網頁瀏覽器） 應該不會用於網域控制站，但數以千計的網域控制站分析揭露許多案例中有特殊權限的使用者使用 Internet Explorer 瀏覽組織內部網路或網際網路。  
  
為先前的錯誤 「 設定 」 一節所述[危害途徑](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)，瀏覽網際網路 （或受感染內部網路），其中一個最有效中的電腦使用高度授權的 account Windows 基礎結構 （這只允許登入本機網域控制站預設帳號） 提供的特殊組織的安全性風險。 透過下載，或惡意程式碼感染的 「 公用程式 」 下載磁碟機，是否攻擊者可以存取完全危害或破壞 Active Directory 環境所需的所有項目。  
  
Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008、 和最新版 Internet Explorer 提供許多保護惡意下載項目，但在大部分案例中網域控制站和特殊權限的帳號有已用來瀏覽網際網路，網域控制站正在執行 Windows Server 2003，或較新的作業系統和瀏覽器所提供的防護已刻意停用。  
  
應該禁止這是第一個網域控制站在網頁瀏覽器，不只原則，但技術控制，並不會允許網域控制站存取網際網路。 如果您的網域控制站需要複製網站上，您應該實作安全連接之間的網站。 雖然這份文件的範圍詳細的設定指示操作，您可以執行控制項，以限制濫用或錯誤設定和後續危害網域控制站的能力的數字。  
  
### <a name="perimeter-firewall-restrictions"></a>周邊防火牆限制  
周邊防火牆應該設定封鎖來自網域控制站輸出連接網際網路。 雖然網域控制站可能需要在網站邊界通訊，可以允許間通訊中所提供的指導方針設定周邊防火牆[如何設定針對網域和信任防火牆](https://support.microsoft.com/kb/179442)Microsoft 支援服務網站上。  
  
### <a name="dc-firewall-configurations"></a>俠防火牆設定  

如之前所述，您應該使用安全性設定精靈擷取網域控制站的 Windows 防火牆使用進階安全性設定設定。 您應該再次檢查以確保符合您組織的需求，防火牆設定並使用 Gpo 執行設定的安全性設定精靈的輸出。  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>防止網頁瀏覽的網域控制站  
防止網域控制站存取網際網路，避免使用網域控制站在網頁瀏覽器，您可以使用 AppLocker 設定、 」 黑色洞 「 proxy 設定] 和 WFAS 設定的組合。  
  


