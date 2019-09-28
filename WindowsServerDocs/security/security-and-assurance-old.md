---
title: 安全性和保證
description: Windows Server 2016 的安全性概觀
ms.custom: na
ms.prod: windows-server
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/27/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.openlocfilehash: f43e638ca2ffbeafcbc8d23e55f5b03911eb0238
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402307"
---
# <a name="security-and-assurance-in-windows-server"></a>Windows Server 的安全性和保證 

>適用於：Windows Server (半年度管道)、Windows Server 2016

>[!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。 您也可以[搜尋這個網站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)以取得特定資訊。

<img src="../media/landing-icons/security.png" style='float:left; padding:.5em;' alt="Icon representing a lock"> 您可以依賴內建於作業系統的多層新防護功能，以進一步防範安全性缺口。 協助阻擋惡意攻擊，並加強虛擬機器、應用程式和資料的安全性。


### <a name="windows-server-security-blog-posthttpsblogstechnetmicrosoftcomwindowsserver20160425ten-reasons-youll-love-windows-server-2016-8-security"></a>[Windows Server 安全性 Blog 文章](https://blogs.technet.microsoft.com/windowsserver/2016/04/25/ten-reasons-youll-love-windows-server-2016-8-security/)
這篇來自 Windows Server 安全性團隊的部落格文章針對 Windows Servers 中的許多改進做出重點介紹，這些改進可提升託管與混合式雲端環境的安全性。

### <a name="datacenter-and-private-cloud-security-bloghttpsblogstechnetmicrosoftcomdatacentersecurity"></a>[資料中心和私用雲端安全性的 Blog](https://blogs.technet.microsoft.com/datacentersecurity/)
這是來自 Microsoft 資料中心和私人雲端安全性團隊之技術性內容的主要部落格網站。                                    

### <a name="addressing-emerging-threats-and-landscape-shiftshttpswwwyoutubecomwatchvb5jmyxywx1kfeatureyoutube"></a>[解決新興威脅和橫向轉移](https://www.youtube.com/watch?v=B5JMYxYWx1k&feature=youtu.be)
在這段 6 分鐘的影片中，Anders Vinberg 會提供 Microsoft 的安全性和保證策略概觀，並討論與安全性有關的產業趨勢和情勢轉移。 接著專注在 Microsoft 針對保護工作負載免於基礎網狀架構影響，以及防範來自特殊權限帳戶的直接攻擊的重點計畫。 最後說明當入侵發生時，新的偵測和鑑識功能會如何更有效地找出威脅。

### <a name="protecting-your-datacenter-and-cloud-from-emerging-threats-blog-posthttpblogstechnetcombwindowsserverarchive20151118protecting-your-datacenter-and-cloud-november-updateaspx"></a>[保護您的資料中心和雲端免于新興威脅的 blog 文章](http://blogs.technet.com/b/windowsserver/archive/2015/11/18/protecting-your-datacenter-and-cloud-november-update.aspx)
本部落格文章討論如何使用 Microsoft 技術保護您的資料中心和雲端投資免於新興威脅。                   

### <a name="security-and-assurance-overview-session-at-ignitehttpchannel9msdncomeventsignite2015brk2482"></a>[Ignite 的安全性和保證總覽課程](http://channel9.msdn.com/events/ignite/2015/brk2482)
此 Ignite 講習將針對持續的威脅、內部人士資料外洩、有組織的網路犯罪，以及保護 Microsoft 雲端平台 (搭配 Azure 的內部部署與已連線服務) 的安全進行討論。 它包含了保護工作負載、大型企業租用戶，以及服務提供者之安全的案例。                                                                   

## <a name="secure-virtualization-with-shielded-vms"></a>透過受防護的 VM 保護虛擬化

### <a name="shielded-vm-in-channel-9httpchannel9msdncomshowsmechanicsintroduction-to-shielded-virtual-machines-in-windows-server-2016"></a>[Channel 9 中的受防護 VM](http://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
逐步解說受防護的 VM 技術與優點。                           

### <a name="shielded-vm-demohttpswwwyoutubecomwatchvxip5qtk-7d8"></a>[受防護的 VM 示範](https://www.youtube.com/watch?v=xip5Qtk-7d8)
這段 4 分鐘的影片說明受防護 VM 的價值，以及受防護 VM 和不受防護 VM 之間的差異。                                   

### <a name="shielded-virtual-machines-in-windows-server-video-walkthroughhttpmicrosoft-cloudcloudguidescomguidesshielded-virtual-machines-in-windows-serverhtm"></a>[Windows Server 中受防護的虛擬機器影片逐步解說](http://microsoft-cloud.cloudguides.com/Guides/Shielded Virtual Machines in Windows Server.htm)
此影片逐步解說示範「主機守護者服務」如何啟用受防護的虛擬機器，以防機密資料受到 Hyper-V 主機系統管理員未經授權的存取。

### <a name="harden-the-fabric-protecting-tenant-secrets-in-hyper-v-ignite-videohttpchannel9msdncomeventsignite2015brk3457"></a>[強化網狀架構：保護 Hyper-V 中的租用戶密碼 (Ignite 影片)](http://channel9.msdn.com/events/ignite/2015/brk3457)

此 Ignite 簡報將討論 Hyper-V、Virtual Machine Manager 中的增強功能，以及一個能啟用受防護 VM 的新守護者伺服器角色。                

### <a name="guarded-fabric-deployment-guidehttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-deploying-hgs-overview"></a>[受防護網狀架構部署指南](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)
本指南針對受防護網狀架構主機和受防護的 VM，提供 Windows Server 及 System Center Virtual Machine Manager 的安裝和驗證資訊。

### <a name="shielded-vm-and-guarded-fabric-in-branch-officeshttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-manage-branch-office"></a>[分公司中受訪護 VM 與 Guarded Fabric](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office)
本指南提供有關在分公司中執行受訪護虛擬機器，以及 Hyper-V 主機可能會有一段時間限制連線至 HGS 等其他遠端案例的最佳作法。

### <a name="shielded-vm-and-guarded-fabric-troubleshooting-guidehttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-troubleshoot-overview"></a>[受防護的 VM 與受防護網狀架構疑難排解指南](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-overview)
本指南提供解決您在受防護 VM 環境可能會遇到之問題的資訊。

### <a name="shielded-vm-articlehttpwindowsitprocomhyper-vsuper-secure-hyper-v-environments-shielded-vms-2016"></a>[受防護的 VM 文章](http://windowsitpro.com/hyper-v/super-secure-hyper-v-environments-shielded-vms-2016)
此白皮書所提供的概觀，說明受防護的 VM 如何提供更強大的整體安全性以避免竄改。                                         

## <a name="privileged-access-management"></a>Microsoft Azure Active Directory
### <a name="securing-privileged-accesshttpstechnetmicrosoftcomwindows-server-docssecuritysecuring-privileged-accesssecuring-privileged-access"></a>[保護特殊權限存取](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access)
說明如何保護特殊權限存取的藍圖。 此藍圖是結合伺服器安全性團隊、Microsoft IT、Azure 團隊和 Microsoft 諮詢服務的專業知識建置而成                           

### <a name="just-in-time-administration-with-microsoft-identity-managerhttpstechnetmicrosoftcomlibrarymt150258aspx"></a>[Microsoft Identity Manager 的 Just in Time Administration](https://technet.microsoft.com/library/mt150258.aspx)
本文討論 Microsoft Identity Manager 中隨附的特性和功能，包括 Just In Time (JIT) 特殊權限存取管理的支援。                                                                    

### <a name="protecting-windows-and-microsoft-azure-active-directory-with-privileged-access-managementhttpchannel9msdncomeventsignite2015brk3873"></a>[使用特殊權限存取管理保護 Windows 和 Microsoft Azure Active Directory](http://channel9.msdn.com/events/ignite/2015/brk3873)
此 Ignite 簡報將涵蓋 Microsoft 在 Windows Server、PowerShell、Active Directory、Identity Manager 和 Azure Active Directory 中的策略與投資，以便透過更強大的驗證功能，以及使用 Just in Time 和 Just Enough Administration (JEA) 管理存取權，來解決系統管理員存取的風險。

### <a name="just-enough-administration-articlehttpakamsjea"></a>[Just Enough Administration 文章](http://aka.ms/JEA)
本文將分享 Just Enough Administration 的願景和技術詳細資料。這是一個 PowerShell 工具組，其設計可協助組織將操作員限制為僅具備執行特定工作所需的存取權，藉以降低風險。

### <a name="just-enough-administration-demo-videohttpswwwyoutubecomwatchvxnbrbky9p20"></a>[Just Enough Administration 示範影片](https://www.youtube.com/watch?v=xnBrbkY9P20)
Just Enough Administration 示範逐步解說。                                                                                                                  
## <a name="credential-protection"></a>認證保護

### <a name="protect-derived-domain-credentials-with-credential-guardhttpsdocsmicrosoftcomwindowssecurityidentity-protectioncredential-guardcredential-guard"></a>[使用 Credential Guard 保護衍生的網域認證](https://docs.microsoft.com/windows/security/identity-protection/credential-guard/credential-guard)
Credential Guard 使用以虛擬化為基礎的安全性來隔離機密資料，使得只有特殊權限的系統軟體可以存取這些資料。 對這些密碼的未經授權存取會導致認證竊取攻擊，例如傳遞雜湊 (Pass-the-Hash) 或傳遞票證 (Pass-The-Ticket)。 Credential Guard 可透過保護 NTLM 密碼雜湊和 Kerberos 票證授權票證來防止這些攻擊。

### <a name="protect-remote-desktop-credentials-with-remote-credential-guardhttpsdocsmicrosoftcomwindowssecurityidentity-protectionremote-credential-guard"></a>[使用 Remote Credential Guard 保護遠端桌面認證](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard)
Remote Credential Guard 可協助您將 Kerberos 要求重新導向回要求連線的裝置，藉以透過遠端桌面連線保護您的認證。 它也會針對遠端桌面工作階段，提供單一登入體驗。                                                                                                        |
### <a name="credential-guard-demo-videohttpswwwyoutubecomwatchveupkogsl7yk"></a>[Credential Guard 示範影片](https://www.youtube.com/watch?v=eUpKOGSl7yk)
這段 5 分鐘的影片示範 Credential Guard 和 Remote Credential Guard。         

## <a name="hardening-the-os-and-applications"></a>強化作業系統和應用程式
### <a name="windows-defender-application-control-wdac-deployment-guidehttpsdocsmicrosoftcomwindowssecuritythreat-protectionwindows-defender-application-controlwindows-defender-application-control"></a>[Windows Defender 應用程式控制 (WDAC) 部署指南](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)
WDAC 是可設定的程式碼完整性 (CI) 原則，可幫助企業控制哪些應用程式在其環境中執行，並執行非執行 Windows 10 的非特定硬體或軟體需求。

### <a name="device-guard-demo-videohttpswwwyoutubecomwatchvf-ptkesjkhi"></a>[Device Guard 示範影片](https://www.youtube.com/watch?v=F-pTkesjkhI)
Device Guard 是 WDAC 與 Hypervisor 保護的程式碼完整性 (HVCI) 的組合。 這段 7 分鐘的影片呈現 Device Guard 及其在 Windows Server 上的使用方式。

### <a name="transport-layer-security-registry-settingshttpsdocsmicrosoftcomwindows-serversecuritytlstls-registry-settings"></a>[傳輸層安全性登錄設定](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)
傳輸層安全性 (TLS) 通訊協定和安全通訊端層 (SSL) 通訊協定其 Windows 實作的支援登錄設定資訊。

### <a name="control-flow-guardhttpsdocsmicrosoftcomwindowsdesktopsecbpcontrol-flow-guard"></a>[控制流程防護](https://docs.microsoft.com/windows/desktop/SecBP/control-flow-guard)
控制流程防護提供內建防護功能，以防範某些類別的記憶體損毀攻擊。

### <a name="windows-defenderhttpstechnetmicrosoftcomwindows-server-docssecuritywindows-defenderwindows-defender-overview-windows-server"></a>[Windows Defender](https://technet.microsoft.com/windows-server-docs/security/windows-defender/windows-defender-overview-windows-server)
Windows Defender 提供主動式偵測功能來封鎖已知的惡意程式碼。 Windows Defender 預設已開啟並最佳化，以支援 Windows Server 中的各種伺服器角色。

## <a name="detecting-and-responding-to-threats"></a>偵測及回應威脅
### <a name="security-threat-analysis-using-microsoft-operations-management-suitehttpschannel9msdncomeventsignite2015brk3464"></a>[使用 Microsoft Operations Management Suite 的安全性威脅分析](https://channel9.msdn.com/events/ignite/2015/brk3464)
此 Ignite 簡報將討論如何使用 Operational Insights 執行安全性威脅分析。

### <a name="microsoft-operations-management-suite-omshttpswwwmicrosoftcomen-usserver-cloudoperations-management-suiteoverviewaspx"></a>[Microsoft Operations Management Suite (OMS)](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx)
Microsoft Operations Management Suite (OMS) 安全性和稽核解決方案可處理內部部署和雲端環境中的安全性記錄檔及防火牆事件，以分析並偵測惡意行為。

### <a name="oms-and-windows-serverhttpswwwyoutubecomwatchv_sadw1dry2k"></a>[OMS 和 Windows Server](https://www.youtube.com/watch?v=_SaDw1dRy2k)
這段 3 分鐘的影片示範 OMS 如何協助偵測 Windows Server 所封鎖的潛在惡意行為。  

### <a name="microsoft-advanced-threat-analyticshttpblogstechnetcombadarchive20150722microsoft-advanced-threat-analytics-coming-next-monthaspx"></a>[Microsoft 進階威脅分析](http://blogs.technet.com/b/ad/archive/2015/07/22/microsoft-advanced-threat-analytics-coming-next-month.aspx)
本部落格文章將討論 Microsoft 進階威脅分析。這是一個內部部署產品，可使用 Active Directory 網路流量和 SIEM 資料，來探索潛在威脅並發出警示。

### <a name="microsoft-advanced-threat-analyticshttpswwwyoutubecomwatchv0na9fetrzfwlistpl8nfc9hageb5izgm8hvmrozethrpbdksw"></a>[Microsoft 進階威脅分析](https://www.youtube.com/watch?v=0nA9FeTRZFw&list=PL8nfc9haGeb5IZGM8HvmRozetHRpBDKSw)
這個 3 分鐘的影片呈現 Microsoft 如何在 Windows Server 中新增威脅分析功能的概觀。                                                                                 |

## <a name="network-security"></a>網路安全性

### <a name="datacenter-firewall-overviewhttpstechnetmicrosoftcomlibrarydn920240aspx"></a>[資料中心防火牆概觀](https://technet.microsoft.com/library/dn920240.aspx)
此概觀將討論資料中心防火牆，這是一個網路層、5-Tuple (通訊協定、來源和目的地連接埠號碼，以及來源和目的地 IP 位址)、可設定狀態、多租用戶的防火牆。

### <a name="whats-new-in-dns-in-windows-serverhttpstechnetmicrosoftcomwindows-server-docsnetworkingdnswhat-s-new-in-dns-server"></a>[Windows Server 中 DNS 的新功能](https://technet.microsoft.com/windows-server-docs/networking/dns/what-s-new-in-dns-server)
此概觀主題簡短說明 DNS 中的新功能，並提供詳細資訊連結。                                                                           

## <a name="mapping-security-features-to-compliance-regulations"></a>將安全性功能對應至相容性規定

相容性是安全性功能的重要層面。 我們除了讓專家建議如何達成您的相容性，以及您信任的相容性顧問眼中的相容性之外，也想要提供初始對應，讓您能夠用來評估 Windows Server。

-   [Hyper-V 受防護的 VM 相容性對應白皮書](https://download.microsoft.com/download/6/D/0/6D06E149-B4C1-4EED-ACD5-DF6066E93CC0/Coalfire_Branded_Hyper_V_Shielded_VMs_Whitepaper_EN_US.pdf)

-   [JEA 和 JIT 相容性對應白皮書](https://download.microsoft.com/download/2/7/A/27A2B5BB-6B52-4482-87C1-DA9D6B6D8C8D/Coalfire_Branded_Privileged_Identity_Manager_Whitepaper_EN_US.pdf)

-   [Device Guard 合規性對應白皮書](https://download.microsoft.com/download/6/9/D/69D9E610-D23C-4F7E-A8CC-D65B87CEB4F8/Coalfire_Branded_Device_Guard_Whitepaper_EN_US.pdf)

-   [Credential Guard 相容性對應白皮書](https://download.microsoft.com/download/8/1/2/812009C9-E4B8-4D4B-AADD-FDC373D0A076/Coalfire_Branded_Credential_Guard_Whitepaper_EN_US.pdf)

-   [Windows Defender 相容性對應白皮書](https://download.microsoft.com/download/C/7/7/C778B7BB-0783-42D7-93A9-B86DFB5A7BAD/Coalfire_Branded_Windows_Defender_Whitepaper_EN_US.pdf)
