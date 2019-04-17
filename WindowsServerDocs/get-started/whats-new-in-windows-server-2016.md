---
title: WindowsServer 2016 的新功能
description: 關於運算、身分識別、管理、自動化、網路功能、安全性、存放裝置的新功能。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 01/05/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2827f332-44d4-4785-8b13-98429087dcc7
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: b504c3396200502a09467ae97a36f9de613e4820
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339386"
---
# WindowsServer 2016 的新功能

>適用於︰WindowsServer 2016

<img src="media/whats-new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">本節內容說明 WindowsServer&reg; 2016 的新功能和變更。 此處所列的新功能和變更是您使用這個版本時最可能帶來最大影響的新功能和變更。  
   
<br>
<br>
<br>
<br>
<br>
## [運算](../virtualization/virtualization.md)  
「虛擬化」領域包含 IT 專業人員可用來設計、部署及維護 WindowsServer 的虛擬化產品與功能。  

### 一般  
由於「Win32 時間」與「Hyper-V 時間同步化」服務中的改進功能，使實體與虛擬機器能從更佳的時間精確度中受益。 WindowsServer 現在可以裝載與未來法規相容的服務，該法規將會要求與 UTC 的時間差異不能超過 1 毫秒。  

### Hyper-V  
-   
  [WindowsServer 2016 的 Hyper-V 新功能](../virtualization/hyper-v/What-s-new-in-Hyper-V-on-Windows.md)。 本主題說明在 WindowsServer 2016、Windows10 上執行的用戶端 Hyper-V，以及 Microsoft Hyper-V Server 2016 中 Hyper-V 角色的新功能和變更。  

-   
  [Windows 容器](https://msdn.microsoft.com/virtualization/windowscontainers/containers_welcome)：WindowsServer 2016 容器支援新增效能改進、簡化網路管理，以及對 Windows10 上 Windows 容器的支援。 如需一些關於容器的額外資訊，請參閱[容器︰Docker、Windows 及趨勢](https://azure.microsoft.com/blog/2015/08/17/containers-docker-windows-and-trends/)。  

### Nano 伺服器  
[Nano 伺服器](getting-started-with-nano-server.md)中的新功能。 Nano 伺服器現在提供建置 Nano 伺服器映像的更新模組，包括實體主機和客體虛擬機器功能之間的進一步分隔，以及針對不同 WindowsServer 版本的支援。   

此外，也針對「修復主控台」提供改進功能，包括輸入和輸出防火牆規則的分隔，以及修復 WinRM 設定的能力。  

### 受防護的虛擬機器  
WindowsServer 2016 提供新的 Hyper-V 型受防護虛擬機器，保護遭入侵光纖中的任何第 2 代虛擬機器。 WindowsServer 2016 中引進的功能如下：  

- 新的「支援加密」模式可提供比一般虛擬機器更多的保護 (但較「防護」模式少)，同時仍支援 vTPM、磁碟加密、即時移轉流量加密，以及其他功能 (包括虛擬機器主控台連線和 Powershell Direct 等直接網狀架構系統管理的便利性)。  

- 完整支援將現有不受防護的第 2 代虛擬機器轉換為受防護的虛擬機器，包括自動化磁碟加密。

- Hyper-V Virtual Machine Manager 現在可以檢視受防護虛擬機器已授權在其上執行的網狀架構，並為網狀架構系統管理員提供開啟受防護虛擬機器的金鑰保護裝置 (KP) 的方式，並檢視允許它在其上執行的網狀架構。  

- 您可以在執行中的「主機守護者服務」上切換證明模式。 現在您可以在較不安全但更為簡單的 Active Directory 型證明與 TPM 型證明之間進行即時切換。  

- 以 Windows PowerShell 為基礎的端對端診斷工具，能夠偵測受防護 Hyper-V 主機與「主機守護者服務」中不正確的設定或錯誤。  

- 修復環境可提供在受防護虛擬機器通常執行的網狀架構中針對它們安全地進行疑難排解和修復的方法，並同時提供和受防護虛擬機器本身相同層級的保護。

- 現有安全 Active Directory 的主機守護者服務支援 – 您可以指示主機守護者服務使用現有 Active Directory 樹系作為其 Active Directory，而不是建立它自己的 Active Directory 執行個體。

如需使用受防護虛擬機器的詳細資料和指示，請參閱[適用於 WindowsServer 2016 的受防護 VM 和受防護網狀架構驗證指南 (TPM)](https://aka.ms/shieldedvms)。  

## [身分識別與存取](../identity/Identity-and-Access.md)  
「身分識別」中的新功能可提升組織保護 Active Directory 環境的能力，並協助他們移轉至僅限雲端的部署和混合式部署，其中有些應用程式與服務會裝載於雲端，而其他則會裝載於內部部署上。  

### Active Directory 憑證服務  
WindowsServer 2016 中的 Active Directory 憑證服務 (AD CS) 增加了對於 TPM 金鑰證明的支援：您現在可以針對金鑰證明使用智慧卡 KSP，而未加入網域的裝置現在可以使用 NDES 註冊來取得可針對 TPM 中的金鑰進行證明的憑證。  

### Active Directory 網域服務  
「Active Directory 網域服務」包含可協助組織確保 Active Directory 環境及為公司裝置與個人裝置提供更佳身分識別管理體驗的改進。 如需詳細資訊，請參閱 [What's new in Active Directory Domain Services (AD DS) in WindowsServer 2016](../identity/whats-new-active-directory-domain-services.md) (WindowsServer 2016 中 Active Directory 網域服務 (AD DS) 的新功能)。   

### Active Directory Federation Services  
Active Directory 同盟服務的新功能。 WindowsServer 2016 中的 Active Directory Federation Services (AD FS) 包含可讓您設定 AD FS 以驗證儲存在「輕量型目錄存取通訊協定」(LDAP) 目錄中之使用者的新功能。 如需詳細資訊，請參閱[適用於 WindowsServer 2016 之 AD FS 的新功能](../identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server.md)。  

### Web 應用程式 Proxy  
最新版的「Web 應用程式 Proxy」著重於可發行和預先驗證更多應用程式及改善使用者體驗的新功能。 請查看新功能的完整清單，其中包括豐富型用戶端應用程式 (例如 Exchange ActiveSync) 的預先驗證，以及讓 SharePoint 應用程式更容易發行的萬用字元網域。 如需詳細資訊，請參閱 [Windows Server 2016 的 Web 應用程式 Proxy](../remote/remote-access/web-application-proxy/web-application-proxy-windows-server.md)。  

##  [系統管理](../administration/manage-windows-server.md)  
「管理和自動化」領域著重在適用於想要執行和管理 WindowsServer 2016 (包括 Windows PowerShell) 之 IT 專業人員的工具及參考資訊。

Windows PowerShell 5.1 包含重要的新功能 (包括支援進行類別開發)，以及新的安全性功能，這些安全性功能可延伸其用途、改善其可用性，並可讓您更輕鬆地全面控制及管理 Windows 型環境。 如需詳細資訊，請參閱 [WMF 5.1 中的新案例及功能](https://docs.microsoft.com/powershell/wmf/5.1/scenarios-features)。

WindowsServer 2016 的新版本包含︰能夠在 Nano 伺服器上本機執行 PowerShell.exe 的能力 (不再僅限遠端)、取代 GUI 的新本機使用者和群組 Cmdlet、新增 PowerShell 偵錯支援，以及在 Nano 伺服器中新增安全性記錄和轉譯及 JEA 的支援。

以下是一些其他新的管理功能：

### Windows Management Framework (WMF) 5 中的 PowerShell 預期狀態設定 (DSC)
Windows Management Framework 5 包含 Windows PowerShell 預期狀態設定 (DSC)、Windows 遠端管理 (WinRM) 和 Windows Management Instrumentation (WMI) 的更新。

如需測試 Windows Management Framework 5 之 DSC 功能的詳細資訊，請參閱[驗證 PowerShell DSC 的功能](https://blogs.msdn.microsoft.com/powershell/2015/07/06/validate-features-of-powershell-dsc/)中討論的部落格文章系列。 若要下載，請參閱 [Windows Management Framework 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure)。

### 軟體探索、安裝和清查的 PackageManagement 整合套件管理
Windows Server 2016 和 Windows 10 包含新的 PackageManagement 功能 (先前稱為 OneGet)，可讓 IT 專業人員或 DevOps 在本機或從遠端自動化軟體探索、安裝及清查 (SDII)，不論安裝程式技術為何，以及軟體位於何處。 

如需詳細資訊，請參閱 [https://github.com/OneGet/oneget/wiki](https://github.com/OneGet/oneget/wiki)。

### 協助數位鑑識並協助減少安全性缺口的 PowerShell 改進
為了協助負責調查受危害系統的團隊 (有時稱為「藍隊」)，我們新增了額外的 PowerShell 記錄和其他數位鑑識功能，並新增功能以協助減少指令碼的弱點，例如限制式 PowerShell 和安全的 CodeGeneration API。

如需詳細資訊，請參閱 [PowerShell ♥ the Blue Team](https://blogs.msdn.microsoft.com/powershell/2015/06/09/powershell-the-blue-team/) (PowerShell 心繫藍隊)。

## [網路功能](../networking/Networking.md)  
此領域提供 IT 專業人員用來設計、部署和維護 WindowsServer 2016 的網路功能產品與功能。  

### 軟體定義的網路
您現在可以對流量進行鏡像處理並路由傳送到新的或現有的虛擬設備。 搭配分散式防火牆和網路安全性群組一起使用，讓您能夠利用類似 Azure 的方式動態區隔及保護工作負載。 其次，您可以使用 System Center Virtual Machine Manager 來部署及管理整個軟體定義網路 (SDN) 堆疊。 最後，您可以使用 Docker 來管理 WindowsServer 容器網路功能，並可以將 SDN 原則與虛擬機器及容器進行關聯。 如需詳細資訊，請參閱[規劃軟體定義網路基礎結構](../networking/sdn/plan/plan-a-software-defined-network-infrastructure.md)。

### TCP 效能改進功能
預設初始壅塞視窗 (ICW) 已經從 4 增加為 10，並已實作 TCP 快速開啟 (TFO)。 TFO 可減少建立 TCP 連線所需的時間量，而增加的 ICW 允許在初始暴增時轉移較大的物件。 這個組合可以大幅減少用戶端與雲端之間轉移網際網路物件所需的時間。

為了在復原封包遺失時改善 TCP 行為，我們已經實作 TCP 結尾遺失探查 (TLP) 和最新通知 (RACK)。 TLP 有助於將重新傳輸逾時 (RTO) 轉換為快速復原，RACK 則降低快速復原重新傳輸遺失封包所需的時間。 

## [安全性和保證](../security/Security-and-Assurance.md)  
包含 IT專業人員可用來部署於您的資料中心和雲端環境中的安全性解決方案與功能。 如需 WindowsServer 2016 中安全性的基本相關資訊，請參閱[安全性和保證](../security/Security-and-Assurance.md)。  

### Just Enough Administration  
WindowsServer 2016 中的 Just Enough Administration 是一種安全性技術，能夠針對可使用 Windows PowerShell 管理的所有項目進行委派管理。 功能包括對下列動作的支援：在網路身分識別下執行、透過 PowerShell Direct 連線、安全地將檔案複製到 JEA 端點 (或是從中複製)，以及將 PowerShell 主控台設定為預設在 JEA 內容中啟動。 如需詳細資訊，請參閱 [GitHub 上的 JEA](https://aka.ms/JEA)。

### Credential Guard
Credential Guard 使用以虛擬化為基礎的安全性來隔離機密資料，使得只有特殊權限的系統軟體可以存取這些資料。 請參閱[使用 Credential Guard 保護衍生的網域認證](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)。

###  Remote Credential Guard
Credential Guard 支援 RDP 工作階段，讓使用者認證保留在用戶端，而不會公開在伺服器端上。 這也會針對遠端桌面提供單一登入。 請參閱[使用 Windows Defender Credential Guard 保護衍生的網域認證](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard)。   

### 裝置防護 (程式碼完整性)
裝置防護提供核心模式程式碼完整性 (KMCI) 和使用者模式程式碼完整性 (UMCI)，方法是建立原則來指定可在伺服器上執行的程式碼。 請參閱 [Windows Defender Device Guard 簡介︰虛擬式安全性和程式碼完整性原則](https://docs.microsoft.com/windows/device-security/device-guard/introduction-to-device-guard-virtualization-based-security-and-code-integrity-policies)。


### Windows Defender  

  [WindowsServer 2016 的 Windows Defender 概觀](../security/windows-defender/windows-defender-overview-windows-server.md)。 WindowsServer 2016 中預設會安裝並啟用 WindowsServer Antimalware，但不會安裝 WindowsServer Antimalware 的使用者介面。 不過，WindowsServer Antimalware 會在沒有使用者介面的情況下更新反惡意程式碼定義並保護電腦。 如果您需要 WindowsServer Antimalware 的使用者介面，您可以使用 [新增角色及功能精靈] 在安裝作業系統之後安裝它。

### 控制流程防護
控制流程防護 (CFG) 是平台安全性功能，建立目的是要對抗記憶體損毀弱點。 如需詳細資訊，請參閱 [Control Flow Guard](https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx) (控制流程防護)。


## [存放](../storage/storage.md)

WindowsServer 2016 中的儲存體包括軟體定義儲存體的新功能和增強功能，以及傳統檔案伺服器的新功能和增強功能。 以下是其中一些新功能。如需詳細增強功能和詳細資訊，請參閱 [WindowsServer 2016 中儲存體的新功能](../storage/whats-new-in-storage.md)。

### 儲存空間 Direct

「儲存空間直接存取」能讓您使用具本機磁碟的伺服器，建置高可用且可調整的儲存空間。 它簡化了軟體定義儲存體系統的部署和管理，也解除了先前使用共用磁碟的叢集儲存空間限制，而能夠使用新的磁碟裝置類型 (例如 SATA SSD 和 NVMe 磁碟裝置)。

如需詳細資訊，請參閱[儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md)。

### 儲存體複本

儲存體複本可在伺服器或叢集之間啟用與存放裝置無關、區塊層級及同步的複寫來進行災害復原，以及在站台之間延伸容錯移轉叢集。 同步複寫能以當機時保持一致的磁碟區啟用對實體站台中資料的鏡像，來確保檔案系統層級零資料遺失。 非同步複寫允許都會範圍外的站台擴充功能，但有資料遺失的可能性。

如需詳細資訊，請參閱[儲存體複本](../storage/storage-replica/storage-replica-overview.md)。

### 存放裝置服務品質 (QoS)

您現在可以在 WindowsServer 2016 中，使用存放裝置服務品質 (QoS) 集中監視端對端儲存體效能，以及使用 Hyper-V 和 CSV 叢集建立管理原則。

如需詳細資訊，請參閱[存放裝置服務品質](../storage/storage-qos/storage-qos-overview.md)。

## [容錯移轉叢集](../failover-clustering/whats-new-in-failover-clustering.md)

WindowsServer 2016 包含的一些新功能和增強功能是針對使用容錯移轉叢集功能分組到單一容錯叢集的多部伺服器。 其中部分新增項目如下；如需更完整的清單，請參閱 [WindowsServer 2016 中容錯移轉叢集的新功能](../failover-clustering/whats-new-in-failover-clustering.md)。

### 叢集作業系統輪流升級

叢集作業系統輪流升級可讓系統管理員將叢集節點的作業系統從 WindowsServer 2012 R2 升級至 WindowsServer 2016，而不需停止 Hyper-V 或「向外延展檔案伺服器」工作負載。 此功能可以避免針對服務等級協定 (SLA) 的停機時間扣分。

如需詳細資訊，請參閱[叢集作業系統輪流升級](../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)。

### 雲端見證

雲端見證是 WindowsServer 2016 中的新類型「容錯移轉叢集」仲裁見證，其利用 Microsoft Azure 作為仲裁點。 雲端見證 (例如任何其他仲裁見證) 會獲得票數，並可以參與仲裁計算。 您可以使用 [設定叢集仲裁精靈] 將雲端見證設定為仲裁見證。

如需詳細資訊，請參閱[部署雲端見證](../failover-clustering/deploy-cloud-witness.md)。

### 健全狀況服務

健全狀況服務可改善「儲存空間直接存取」叢集上叢集資源的日常監視、作業和維護體驗。

如需詳細資訊，請參閱[健全狀況服務](../failover-clustering/health-service-overview.md)。

## 應用程式開發

### Internet Information Services (IIS) 10.0
Windows Server 2016 的 IIS 10.0 Web 伺服器提供新功能，包括︰

- 支援網路堆疊 HTTP/2 通訊協定，並整合 IIS 10.0，讓 IIS 10.0 網站針對支援的設定自動服務 HTTP/2 要求。 這允許許多 HTTP/1.1 增強功能，例如更有效率重複使用連線和降低延遲、改善網頁載入時間。 
- 在 Nano Server 中執行並管理 IIS 10.0 的能力。 請參閱 [Nano Server 上的 IIS](iis-on-nano-server.md)。
- 支援萬用字元主機標頭，讓系統管理員設定網域的網頁伺服器，並讓網頁伺服器服務子網域的要求。
- 新 PowerShell 模組 (IISAdministration) 來管理 IIS。 

如需詳細資訊，請參閱 [IIS](https://iis.net/learn)。

### 分散式交易協調器 (MSDTC)
在 Microsoft Windows 10 和 Windows Server 2016 中增加三項新功能：

- 新的資源管理員重新加入介面可供資源管理員於資料庫因為發生錯誤而重新開機之後判斷不確定交易的結果。 如需詳細資訊，請參閱 [IResourceManagerRejoinable::Rejoin](https://msdn.microsoft.com/en-us/library/mt203799(v=vs.85).aspx)。

- DSN 名稱限制從 256 位元組增加到 3072 位元組。 如需詳細資訊，請參閱 [IDtcToXaHelperFactory::Create](https://msdn.microsoft.com/en-us/library/ms686861(v=vs.85).aspx)，[IDtcToXaHelperSinglePipe::XARMCreate](https://msdn.microsoft.com/en-us/library/ms679248(v=vs.85).aspx) 或[IDtcToXaMapper::RequestNewResourceManager](https://msdn.microsoft.com/en-us/library/ms680310(v=vs.85).aspx)。

- 已改善追蹤可讓您設定登錄金鑰，將映像檔路徑包含在追蹤記錄檔名稱，以決定檢查哪個追蹤記錄檔。 如需設定 MSDTC 追蹤，請參閱[如何在 Windows 電腦上啟用 MS DTC 診斷追蹤](https://support.microsoft.com/en-us/kb/926099)。



## 另請參閱  
-   [版本資訊：Windows Server 2016 的重要問題](Windows-Server-2016-GA-Release-Notes.md)  

