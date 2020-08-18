---
title: 為何特殊權限存取工作站可以協助保護您的組織
description: PAW 如何增加您組織的安全性情勢
ms.topic: article
ms.assetid: 93589778-3907-4410-8ed5-e7b6db406513
ms.date: 03/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 014e19088394135c00d1df63a46ba74f400fa411
ms.sourcegitcommit: 08da40966c5d633f8748c8ae348f12656a54d3b2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/12/2020
ms.locfileid: "88140305"
---
# <a name="privileged-access-workstations"></a>特殊權限存取工作站

>適用於：Windows Server

特殊權限存取工作站 (PAW) 會針對機密工作，提供一個可免受網際網路攻擊和威脅影響的專用作業系統。 將這些機密工作和帳戶與日常使用的工作站和裝置分開可提供非常強大的防護，以防止網路釣魚攻擊、應用程式和作業系統弱點、多種模擬攻擊，以及認證遭竊攻擊，例如按鍵輸入記錄、[傳遞雜湊](https://aka.ms/pth)和傳遞票證。

## <a name="what-is-a-privileged-access-workstation"></a>什麼是特殊權限存取工作站 (PAW)？

簡單地說，PAW 是一個經過強化且已鎖定的工作站，其設計是為了針對機密帳戶和工作提供高安全性保證。  對於身分識別系統、雲端服務和私人雲端網狀架構的系統管理，以及機密的商務功能，建議使用 PAW。

> [!NOTE]
> PAW 架構不需要帳戶與工作站的 1:1 對應，但這是一般設定。 PAW 會建立可由一個或多個帳戶所使用的受信任工作站環境。

為了提供最大的安全性，PAW 應該一律執行最新且最安全的作業系統：︰Microsoft 強烈建議使用 Windows 10 企業版，其中包含一些其他版本未提供的額外安全性功能 (特別是 [Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard) 和 [Device Guard](/windows/security/threat-protection/device-guard/introduction-to-device-guard-virtualization-based-security-and-windows-defender-application-control))。

> [!NOTE]
> 無法存取 Windows 10 企業版的組織可以使用 Windows 10 專業版，其中包含 PAW 的許多重要基本技術，包括信任式開機、BitLocker 和遠端桌面。  教育客戶可以使用 Windows 10 教育版。  Windows 10 家用版不應用於 PAW。
>
> 如需不同版本 Windows 10 的比較表，請參閱[本文](https://www.microsoft.com/WindowsForBusiness/Compare)。

PAW 安全性控制項著重在緩和最大的影響力以及高可能性的危害風險。 這些包括緩和對環境的攻擊，以及緩和可能會隨著時間降低 PAW 控制項效率的風險︰

* **網際網路攻擊** - 大部分的攻擊都直接或間接地源自於網際網路來源，並使用網際網路進行命令和控制項的滲透 (C2)。 將 PAW 與開放式網際網路隔開是確保 PAW 不會受到危害的關鍵要素。
* **可用性風險** - 如果 PAW 太困難而無法用於日常工作，系統管理員就會想建立因應措施，讓他們的工作更輕鬆。 通常這些因應措施會使系統管理工作站和帳戶暴露在明顯的安全性風險之下，因此，要求並授權 PAW 使用者安全地緩和這些可用性問題相當重要。 聆聽他們的意見反映、安裝執行其工作所需的工具和指令碼，並確保所有系統管理人員都了解為什麼需要使用 PAW、什麼是 PAW，以及如何正確且成功地使用 PAW，可達到這個目的。
* **環境風險** - 由於環境中的其他許多電腦和帳戶直接或間接地暴露在網際網路風險之下，因此必須保護 PAW 免於在實際執行環境中遭受來自受危害資產的攻擊。 這必須將可存取 PAW 的管理工具和帳戶減到最少，來保護及監視這些專業工作站。
* **供應鏈竄改** - 無法在軟硬體的供應鏈中移除竄改的所有可能風險時，採取幾個關鍵動作可緩和攻擊者現成可用的重要攻擊方式。 這包括驗證所有安裝媒體的完整性 ([乾淨來源準則](https://aka.ms/cleansource))，以及使用受信任且信譽良好的軟硬體供應商。
* **實體攻擊** - 由於 PAW 可能在實體上可以移動，且可在實體上安全的設施外部使用，因此必須受到保護，以免遭到對電腦運用未授權實體存取的攻擊。

> [!NOTE]
> PAW 將不會保護環境，免於受到已經透過 Active Directory 樹系取得系統管理存取權的敵人攻擊。
> 因為許多現有的 Active Directory 網域服務實作在認證遭竊的風險方面都已經行之多年，因此組織應該假設入侵，並考慮他們可能還未偵測到網域或企業系統管理員認證受到危害的可能性。 懷疑網域遭到入侵的組織應該考慮使用專業的事件回應服務。
>
> 如需有關回應與復原指導方針的詳細資訊，請參閱 [Mitigating Pass-the-Hash and Other Credential Theft](https://aka.ms/pth) (第 2 版) 的 "Respond to suspicious activity" 和 "Recover from a breach" 章節。
>
> 如需詳細資訊，請瀏覽 [Microsoft 的事件回應和復原服務](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx)頁面。

### <a name="paw-hardware-profiles"></a>PAW 硬體設定檔

系統管理人員也是標準的使用者，他們需要 PAW，也需要標準的使用者工作站，才能檢查電子郵件、瀏覽網頁，以及存取公司的企業營運應用程式。  確保系統管理員可以同時保持生產力與安全性對成功部署任何 PAW 至關緊要。  如果使用者希望提升生產力 (即使是以不安全的方式進行)，則會放棄大幅限制生產力的安全解決方案。

為了平衡安全性與生產力的需求，Microsoft 建議使用其中一個 PAW 硬體設定檔︰

* **專用硬體** - 使用者工作與系統管理工作使用的不同專用裝置。
* **同時使用** - 可以利用作業系統或展示虛擬化同時執行使用者工作和系統管理工作的單一裝置。

組織可能僅使用一個設定檔或同時使用兩個設定檔。 硬體設定檔之間沒有任何互通性考量，因此組織具備使硬體設定檔符合指定系統管理員特定需求和情況的彈性。

> [!NOTE]
> 在所有這些情況下，都請務必發給系統管理人員一個不同於指定系統管理帳戶的標準使用者帳戶。 系統管理帳戶應僅用於 PAW 系統管理作業系統。

此表格從操作易用性以及生產力和安全性的觀點，摘要說明每個硬體設定檔相對的優缺點。  這兩種硬體方法都會針對認證竊取和重複使用，為系統管理帳戶提供強大的安全性。

|**案例**|**優點**|**缺點**|
|--------|---------|-----------|
|專用硬體|-   工作敏感度訊號強<br />-   安全隔離最強|-   額外的桌面空間<br />-   額外的重量 (用於遠端工作)<br />-   硬體成本|
|同時使用|-   硬體成本較低<br />-   單一裝置體驗|-   共用單一鍵盤/滑鼠產生意外錯誤/風險的風險|

本指導方針包含適用於專用硬體方法之 PAW 設定的詳細指示。 如果您有同時使用硬體設定檔的需求，可以根據本指導方針自行調整指示，或雇用專業服務組織協助調整，例如 Microsoft。

#### <a name="dedicated-hardware"></a>專用硬體

在此情況下，PAW 用於系統管理，並完全與用於日常活動 (例如電子郵件、文件編輯和開發工作) 的電腦分開。 所有系統管理工具和應用程式都安裝在 PAW 上，而且所有生產力應用程式都安裝在標準使用者工作站上。 本指導方針中的逐步指示是以此硬體設定檔為基礎。

#### <a name="simultaneous-use---adding-a-local-user-vm"></a>同時使用 - 新增本機使用者 VM

在這個同時使用的情況下，一部電腦同時用於系統管理工作和日常活動，例如電子郵件、文件編輯，以及開發工作。 在此設定中，使用者作業系統可在中斷連線時使用 (例如編輯文件，以及處理本機快取的電子郵件)，但是需要可配合此中斷連線狀態的硬體和支援程序。

![此圖顯示在這個同時使用的情況下，一部電腦同時用於系統管理工作和日常活動，例如電子郵件、文件編輯，以及開發工作](../media/privileged-access-workstations/PAWFig10.JPG)

實體硬體會在本機執行兩個作業系統︰

* **系統管理作業系統** - 實體主機在用於系統管理工作的 PAW 主機上執行 Windows 10
* **使用者作業系統** - Windows 10 用戶端 Hyper-V 虛擬機器客體執行公司映像

透過 Windows 10 Hyper-V，也執行 Windows 10 的客體虛擬機器可能會有豐富的使用者經驗，包括聲音、視訊，以及網際網路通訊應用程式，例如商務用 Skype。

在此設定中，不需要系統管理權限的日常工作會在具備一般 Windows 10 公司映像的使用者作業系統虛擬機器中完成，而且不會受制於套用至 PAW 主機的限制。 所有系統管理工作都會在系統管理作業系統上完成。

若要進行此設定，請遵循本指導方針中對於 PAW 主機的指示，新增用戶端 Hyper-V 功能、建立使用者 VM，然後在使用者 VM 上安裝 Windows 10 公司映像。

如需有關這項功能的詳細資訊，請閱讀[用戶端 Hyper-V](/virtualization/hyper-v-on-windows/index) 文件。 請注意，客體虛擬機器中的作業系統必須先根據 [Microsoft 產品授權](https://www.microsoft.com/Licensing/product-licensing/products.aspx)獲得授權，在[這裡](https://download.microsoft.com/download/9/8/D/98D6A56C-4D79-40F4-8462-DA3ECBA2DC2C/Licensing_Windows_Desktop_OS_for_Virtual_Machines.pdf)也有相關說明。

#### <a name="simultaneous-use---adding-remoteapp-rdp-or-a-vdi"></a>同時使用 - 新增 RemoteApp、RDP 或 VDI

在這個同時使用的情況下，一部電腦同時用於系統管理工作和日常活動，例如電子郵件、文件編輯，以及開發工作。 在此設定中，系統會集中部署並管理使用者作業系統 (在雲端上或在您的資料中心)，但在中斷連線時無法使用。

![此圖顯示在這個同時使用的情況下，一部電腦同時用於系統管理工作和日常活動，例如電子郵件、文件編輯，以及開發工作](../media/privileged-access-workstations/PAWFig11.JPG)

實體硬體會針對系統管理工作，在本機執行單一 PAW 作業系統，並連絡 Microsoft 或協力廠商遠端桌面服務，以取得使用者應用程式，例如電子郵件、文件編輯，以及企業營運應用程式。

在此設定中，不需要系統管理權限的日常工作會在遠端作業系統和應用程式中完成，而且這些作業系統和應用程式不會受制於套用至 PAW 主機的限制。 所有系統管理工作都會在系統管理作業系統上完成。

若要進行此設定，請遵循本指導方針中對於 PAW 主機的指示，允許遠端桌面服務的網路連線，然後將捷徑新增至 PAW 使用者的桌面，以存取應用程式。 遠端桌面服務可以使用多種方式託管，包括︰

* 現有的遠端桌面或 VDI 服務 (內部部署或雲端)
* 您在內部部署或雲端安裝的新服務
* 使用預先設定的 Office 365 範本或您自己的安裝映像的 Azure RemoteApp

如需有關 Azure RemoteApp 的詳細資訊，請瀏覽[本頁](https://www.remoteapp.windowsazure.com)。

## <a name="how-microsoft-is-using-administrative-workstations"></a>Microsoft 如何使用系統管理工作站

Microsoft 在我們的系統內部以及我們的客戶使用 PAW 架構方法。 Microsoft 會使用系統管理工作站內部的一些功能，包括 Microsoft IT 基礎結構的系統管理、Microsoft 雲端網狀架構基礎結構的開發與操作，以及其他高價值的資產。

本指導方針是直接以特殊權限存取工作站 (PAW) 參考架構為基礎，我們網頁安全性的專業服務團隊會部署這個架構來保護客戶免受網路安全性攻擊。 系統管理工作站也是網域系統管理工作、增強型安全性系統管理環境 (ESAE) 系統管理樹系參考架構最強防護的一個關鍵要素。

如需有關 ESAE 系統管理樹系的詳細資訊，請參閱[保護特殊權限存取的參考資料](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach)中的 *ESAE 系統管理的樹系設計方法*一節。

## <a name="architecture-overview"></a>架構概觀

下圖說明系統管理 (高度機密的工作) 的個別「通道」，此通道是透過維護各個專用的系統管理帳戶及工作站所建立。

![此圖顯示系統管理 (高度機密的工作) 的個別「通道」，此通道是透過維護各個專用的系統管理帳戶及工作站所建立](../media/privileged-access-workstations/PAWFig1.JPG)

此架構方法建置在 Windows 10 [Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard) 和 [Device Guard](/windows/security/threat-protection/device-guard/introduction-to-device-guard-virtualization-based-security-and-windows-defender-application-control) 功能中的防護之上，而且超越機密帳戶和工作的這些防護。

這種方法適合可存取高價值資產的帳戶︰

* **系統管理權限** - PAW 可針對高影響的 IT 系統管理角色和工作，提供更高的安全性。 這個架構可以套用到許多系統類型的系統管理，包括 Active Directory 網域與樹系、Microsoft Azure Active Directory 租用戶、Office 365 租用戶、程序控制網路 (PCN)、監管控制及資料取得 (SCADA) 系統、自動提款機 (ATM)，以及銷售點 (PoS) 裝置。
* **高機密性資訊工作者** - PAW 中使用的方法也可以針對高機密性資訊工作者的工作和人員提供防護，例如，涉及公告前合併和收購活動、發行前版本財務報表、組織的社交媒體目前狀態、行政溝通、無專利的商業機密、機密研究，或其他專屬或機密資料的工作和人員。 本指導方針並不會深入討論這些資訊工作者案例的設定，也不會將此案例包含在技術指導中。

    > [!NOTE]
    > Microsoft IT 使用 PAW (內部稱為「安全的系統管理工作站」或 SAW) 管理在 Microsoft 內部對於內部高價值系統的安全存取。 本指導方針在稍後的「Microsoft 如何使用系統管理工作站」一節中，對於 PAW 在 Microsoft 的使用方式提供其他詳細資訊。 如需有關這個高價值資產環境方法的詳細資訊，請參閱[使用安全的系統管理工作站保護高價值的資產](/previous-versions/mt186538(v=technet.10))一文。

本文件將說明為什麼建議使用這個做法保護高影響力的特殊權限帳戶、這些 PAW 解決方案在保護系統管理權限時看起來像什麼，以及如何快速部署 PAW 解決方案以管理網域和雲端服務。

本文件針對實作數個 PAW 設定提供詳細的指導方針，並包含詳細的實作指示，協助您開始保護一般高影響力帳戶︰

* [**階段 1 - 立即部署 Active Directory 系統管理員**](#phase-1-immediate-deployment-for-active-directory-administrators)：此階段會快速提供一個可以保護內部部署網域和樹系系統管理角色的 PAW
* [**階段 2 - 將 PAW 延伸至所有系統管理員**](#phase-2-extend-paw-to-all-administrators)：此階段可讓系統管理員保護雲端服務，例如 Office 365 和 Azure、企業伺服器、企業應用程式，以及工作站
* [**階段 3 - 進階的 PAW 安全性**](#phase-3-extend-and-enhance-protection)：此階段會討論 PAW 安全性的其他防護與考量

### <a name="why-dedicated-workstations"></a>為什麼選擇專用的工作站？

目前的組織威脅環境充斥著複雜的網路釣魚和其他網際網路攻擊，這些攻擊會因為網際網路所暴露的帳戶及工作站，導致持續危害安全性的風險。

此威脅環境在設計高價值資產 (例如，系統管理帳戶和機密的企業資產) 的防護時，要求組織採用「假設入侵」安全性情勢。 這些高價值的資產必須受到保護，以防遭受直接網際網路威脅，以及從環境中的其他工作站、伺服器和裝置發動的攻擊。

![此圖顯示攻擊者取得使用機密認證所在使用者工作站的控制權時，受管理資產的風險](../media/privileged-access-workstations/PAWFig2.JPG)

此圖說明攻擊者取得使用機密認證所在使用者工作站的控制權時，受管理資產的風險。

控制作業系統的攻擊者有多種方式可以暗中取得對工作站上所有活動的存取權，並模擬合法的帳戶。 各種已知和未知的攻擊技術都可以用來取得這個層級的存取權。 日漸增加的網路攻擊數量和複雜度使得人們不得不將機密帳戶的分隔概念延伸至完全分隔的用戶端作業系統。 如需有關這類攻擊的詳細資訊，請瀏覽[傳遞雜湊網站](https://www.microsoft.com/pth)以取得知識性白皮書、影片等等。

PAW 方法是既定建議做法的延伸，也就是為系統管理人員使用不同的系統管理員帳戶和使用者帳戶。 此做法會使用個別指派的系統管理帳戶，而且此帳戶與使用者的標準使用者帳戶完全不同。 PAW 為這些機密帳戶提供值得信任的工作站，以便建置在該帳戶分隔做法上。

本 PAW 指導方針旨在協助您實作這項功能來保護高價值帳戶，例如，高權限的 IT 系統管理員和高機密性的商務帳戶。 本指導方針可協助您︰

* 限制僅將認證暴露給受信任的主機
* 為系統管理員提供一個高安全性的工作站，讓他們可以輕鬆地執行系統管理工作。

限制機密帳戶僅使用強化的 PAW 是對這些帳戶的直接防護，這是對於系統管理員高度可用，敵人卻難以擊敗的做法。

### <a name="alternate-approaches"></a>替代方法

本節包含替代方法安全性與 PAW 的對照，以及如何在 PAW 架構中正確整合這些方法的相關資訊。 所有這些方法在個別實作時都有顯著的風險，但是在某些情況下，可以增加 PAW 實作的價值。

#### <a name="credential-guard-and-windows-hello-for-business"></a>Credential Guard 與 Windows Hello 企業版

Windows 10 中推出的 [Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard) 使用硬體和虛擬化架構的安全性保護衍生的認證，藉此減輕常見的認證竊取攻擊，例如傳遞雜湊。 [Windows Hello 企業版](https://aka.ms/passport)所用憑證的私密金鑰也可以受到信賴平台模組 (TPM) 硬體的保護。

這些是功能強大的防護措施，但是即使認證受到 Credential Guard 或 Windows Hello 企業版的保護，工作站仍然可能容易遭受特定攻擊。 攻擊可能包括濫用權限和直接使用受危害裝置中的認證、在啟用 Credential Guard 之前重複使用先前遭竊的認證，以及濫用工作站上的管理工具和弱式應用程式設定。

本節中的 PAW 指導方針包含在高機密性帳戶和工作上使用許多這些技術。

#### <a name="administrative-vm"></a>系統管理 VM

系統管理虛擬機器 (Admin VM) 是一個專用的作業系統，適用於標準使用者桌面上託管的系統管理工作。 雖然這個方法在為系統管理工作提供專用的作業系統上類似於 PAW，但是有一個嚴重的缺點，亦即，系統管理 VM 基於安全性，相依於標準使用者桌面。

下圖說明攻擊者能夠在使用者工作站上，使用系統管理 VM 依照控制鏈到達感興趣的目標物件，而難以針對反向設定建立路徑。

PAW 架構不允許在使用者工作站上裝載系統管理 VM，但是可以在系統管理 PAW 上託管具有標準公司映像的使用者 VM，以便為人員提供單一電腦上的所有責任。

![PAW 架構圖](../media/privileged-access-workstations/PAWFig9.JPG)

#### <a name="shielded-vm-based-paws"></a>受防護 VM 架構下的 PAW

系統管理 VM 模型的安全變體是使用 [受防護的虛擬機器](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)，在使用者 VM 旁裝載一或多個系統管理 VM。
受防護的 VM 是設計用來執行安全的工作負載，用在實體機器的標準使用者桌面上可能執行不受信任使用者或程式碼的環境中。
受防護的 VM 有虛擬 TPM (可讓它加密其本身的待用資料)，並可停用數個系統管理控制項 (例如基本主控台存取、PowerShell Direct) 和偵錯 VM 的能力，進一步隔離 VM 與標準使用者桌面和其他 VM。
受防護 VM 的金鑰會儲存在信任的金鑰管理伺服器上，後者會先要求實體裝置證明其身分識別和健康狀態，再釋放金鑰以啟動 VM。
這可確保受防護的 VM 只能在預定的裝置上啟動，而且這些裝置是執行已知且受信任的軟體設定。

因為受防護的 VM 會彼此隔離，而且是標準使用者桌面，所以可接受在單一主機上執行多個受防護 PAW VM，即使這些系統管理 VM 管理不同層也不成問題。

如需詳細資訊，請參閱下面的[使用受防護網狀架構部署 PAW](#deploy-paws-using-a-guarded-fabric) 一節。

#### <a name="jump-server"></a>跳躍伺服器

系統管理的「跳躍伺服器」架構會設定小量的系統管理主控台伺服器，並限制人員將這些伺服器用於系統管理工作。 這通常是以遠端桌面服務、協力廠商展示虛擬化解決方案，或虛擬桌面基礎結構 (VDI) 技術為基礎。

提出此方法通常是為了降低系統管理的風險，並提供一些安全性保證，但跳躍伺服器方法本身容易受到特定攻擊，因為它違反了[「乾淨來源」準則](../securing-privileged-access/securing-privileged-access-reference-material.md#clean-source-principle)。 乾淨來源準則要求所有安全相依項目必須和要保護的物件一樣值得信任。

![此圖顯示簡單的控制項關聯性](../media/privileged-access-workstations/PAWFig3.JPG)

此圖說明簡單的控制項關聯性。 控制物件的任何主體都是該物件的安全相依性。 如果敵人可以控制目標物件的安全相依項目 (主體)，則可以控制該物件。

跳躍伺服器上的系統管理工作階段會依賴存取該跳躍伺服器的本機電腦完整性。 如果此電腦是容易受到網路釣魚攻擊和其他網際網路型攻擊的使用者工作站，則系統管理工作階段也會受到這些風險的影響。

![此圖顯示攻擊者如何依照已建立的控制鏈，到達感興趣的目標物件](../media/privileged-access-workstations/PAWFig4.JPG)

上圖說明攻擊者如何依照已建立的控制鏈，到達感興趣的目標物件。

雖然有些進階的安全性控制項 (例如多重要素驗證) 可能會增加攻擊者從使用者工作站接管此系統管理工作階段的困難度，但是在攻擊者擁有來源電腦的系統管理存取權 (例如，將非法的命令插入合法的工作階段、劫持合法的處理程序等等) 時，沒有任何安全性功能可以完全防範技術攻擊。

本 PAW 指導方針中的預設組態會在 PAW 上安裝系統管理工具，但是如果需要，也可以新增跳躍伺服器架構。

![此圖顯示如何反轉控制項關聯性，並從系統管理工作站存取使用者應用程式，讓攻擊者沒有目標物件的路徑](../media/privileged-access-workstations/PAWFig5.JPG)

本圖顯示如何反轉控制項關聯性，並從系統管理工作站存取使用者應用程式，讓攻擊者沒有目標物件的路徑。 使用者跳躍伺服器仍會暴露在風險之下，因此，仍然應該針對網際網路對向電腦套用適當的保護控制項、偵測控制項，以及回應處理程序。

此設定會要求系統管理員仔細遵循操作實務，以確保他們不會不小心將系統管理員認證輸入到其桌面上的使用者工作階段中。

![此圖顯示從 PAW 存取系統管理跳躍伺服器時，不會在系統管理資產中新增攻擊者路徑的方式](../media/privileged-access-workstations/PAWFig6.JPG)

本圖顯示從 PAW 存取系統管理跳躍伺服器時，不會在系統管理資產中新增攻擊者路徑的方式。 在此情況下，具有 PAW 的跳躍伺服器可讓您彙總位置的數目，以監視系統管理活動並散佈系統管理應用程式和工具。 如此可增加一些設計上的複雜性，但是如果在 PAW 實作中使用大量的帳戶和工作站，則可以簡化安全性監視和軟體更新。 必須建置跳躍伺服器並設定為與 PAW 類似的安全性標準。

#### <a name="privilege-management-solutions"></a>權限管理解決方案

權限管理解決方案是對隨需的不同權限或特殊權限帳戶提供暫時存取權的應用程式。 權限管理解決方案是完整策略的一個非常珍貴的元件，可保護特殊權限存取的安全，並為系統管理活動提供非常重要的能見度和權責。

這些解決方案通常會使用彈性的工作流程授與存取權，而且其中許多解決方案都有其他安全性特色與功能，例如，管理服務帳戶密碼以及整合系統管理跳躍伺服器。 市面上有許多解決方案都提供權限管理功能，其中一個是 Microsoft Identity Manager (MIM) 特殊權限存取管理 (PAM)。

Microsoft 建議使用 PAW 存取權限管理解決方案。 對於這些解決方案的存取權應該僅授與給 PAW。 Microsoft 不建議使用這些解決方案取代 PAW，因為從可能受到危害的使用者桌面使用這些解決方案存取權限違反[乾淨來源](https://aka.ms/cleansource)準則，如下圖所示︰

![此圖顯示 Microsoft 不建議使用這些解決方案取代 PAW，因為從可能受到危害的使用者桌面使用這些解決方案存取權限違反乾淨來源準則](../media/privileged-access-workstations/PAWFig7.JPG)

提供 PAW 存取這些解決方案可讓您同時獲得 PAW 和權限管理解決方案的安全優勢，如本圖所示︰

![此圖顯示提供 PAW 存取這些解決方案如何讓您同時獲得 PAW 和權限管理解決方案的安全優勢](../media/privileged-access-workstations/PAWFig8.JPG)

> [!NOTE]
> 這些系統應該在所管理的最高權限層加以分類，並在該安全性層級以上受到保護。 這些通常會設定為管理第 0 層解決方案及第 0 層資產，且應該在第 0 層加以分類。
> 如需有關階層模型的詳細資訊，請參閱 [https://aka.ms/tiermodel](https://aka.ms/tiermodel) 如需有關第 0 層群組的詳細資訊，請參閱[保護特殊權限存取的參考資料](../securing-privileged-access/securing-privileged-access-reference-material.md)中的第 0 層對應項。

如需有關部署 Microsoft Identity Manager (MIM) 特殊權限存取管理 (PAM) 的詳細資訊，請參閱 [https://aka.ms/mimpamdeploy](https://aka.ms/mimpamdeploy)

## <a name="paw-scenarios"></a>PAW 案例

本節包含案例中應該套用此 PAW 指導方針的指導方針。 在所有案例中，都應該訓練系統管理員只使用 PAW 執行遠端系統的支援。 為鼓勵成功且安全的使用方式，也應該鼓勵所有 PAW 使用者提供意見反應以改善 PAW 體驗，因此應該仔細檢閱此意見反應，才能與 PAW 程式整合在一起。

在所有案例中，可使用稍後階段中的其他強化功能，以及本指導方針中不同的硬體設定檔，以符合角色的可用性或安全性需求。

> [!NOTE]
> 本指導方針會明確區分需要存取網際網路上的特定服務 (例如 Azure 和 Office 365 系統管理入口網站)，以及需要存取所有主機和服務的「開放式網際網路」。

如需有關階層指定的詳細資訊，請參閱[階層模型頁面](https://aka.ms/tiermodel)。

|**案例**|**使用 PAW？**|**範圍和安全性考量**|
|---------|--------|---------------------|
|Active Directory 管理員 - 第 0 層|是|使用階段 1 指導方針建置的 PAW 對於此角色已經足夠。<p>-   可以新增系統管理樹系，以便為此案例提供最強的防護。 如需有關 ESAE 系統管理樹系的詳細資訊，請參閱 [ESAE 系統管理樹系設計方法](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach)<br />-   PAW 可以用來管理多個網域或多個樹系。<br />-   如果在基礎結構即服務 (IaaS) 或內部部署虛擬化解決方案上裝載網域控制站，您應該為那些解決方案的系統管理員優先實作 PAW。|
|Azure IaaS 和 PaaS 服務的系統管理員 - 第 0 層或第 1 層 (請參閱「範圍和設計考量」)|是|使用階段 2 中提供的指導方針建置的 PAW 對於此角色已經足夠。<p>-   PAW 應該至少用於全域管理員和訂閱計費管理員。 您也應該將 PAW 用於重要或機密伺服器的委派系統管理員。<br />-   PAW 應該用於管理針對雲端服務 (例如 [Azure AD Connect](/azure/active-directory/hybrid/whatis-hybrid-identity) 和 Active Directory 同盟服務 (ADFS)) 提供目錄同步作業和身分識別同盟的作業系統與應用程式。<br />-   輸出網路限制必須允許使用階段 2 中的指導方針，僅與已授權的雲端服務連線。 不應該從 PAW 允許開放式網際網路存取。<br />- 應在工作站上設定 Windows Defender 惡意探索防護 **附註：**   如果網域控制站或其他第 0 層主機位於訂閱中，則會將該訂閱視為樹系的第 0 層。 如果在 Azure 中沒有裝載第 0 層伺服器，則訂閱是第 1 層。|
|管理 Office 365 租用戶 <br />- 第 1 層|是|使用階段 2 中提供的指導方針建置的 PAW 對於此角色已經足夠。<p>-   PAW 應該至少用於訂閱計費管理員、全域管理員、Exchange 系統管理員、SharePoint 系統管理員，以及使用者管理管理員角色。 您也應該認真考慮將 PAW 用於高度重要或敏感資訊的委派系統管理員。<br />- 應在工作站上設定 Windows Defender 惡意探索防護。<br />-   輸出網路限制必須允許使用階段 2 中的指導方針，僅與 Microsoft 服務連線。 不應該從 PAW 允許開放式網際網路存取。|
|其他 IaaS 或 PaaS 雲端服務系統管理員<br />- 第 0 層或第 1 層 (請參閱「範圍和設計考量」)|是|使用階段 2 中提供的指導方針建置的 PAW 對於此角色已經足夠。<p>-   PAW 應該用於對雲端託管 VM 具有系統管理權限的任何角色，包括能夠安裝代理程式；匯出硬碟檔案；或存取儲存作業系統、機密資料或業務關鍵資料所在硬碟的儲存裝置。<br />-   輸出網路限制必須允許使用階段 2 中的指導方針，僅與 Microsoft 服務連線。 不應該從 PAW 允許開放式網際網路存取。<br />- 應在工作站上設定 Windows Defender 惡意探索防護。 **注意：** 如果網域控制站或其他第 0 層主機位於訂閱中，則該訂閱為樹系的第 0 層。 如果在 Azure 中沒有裝載第 0 層伺服器，則訂閱是第 1 層。|
|虛擬化系統管理員<br />- 第 0 層或第 1 層 (請參閱「範圍和設計考量」)|是|使用階段 2 中提供的指導方針建置的 PAW 對於此角色已經足夠。<p>-   PAW 應該用於對 VM 具有系統管理權限的任何角色，包括能夠安裝代理程式；匯出虛擬硬碟檔案；或存取儲存客體作業系統資訊、機密資料或業務關鍵資料所在硬碟的儲存裝置。 **注意：** 如果網域控制站或其他第 0 層主機位於訂閱中，則會將虛擬化系統 (及其系統管理員) 視為樹系的第 0 層。 如果在虛擬化系統中沒有裝載第 0 層伺服器，則訂閱是第 1 層。|
|伺服器維護系統管理員<br />- 第 1 層|是|使用階段 2 中提供的指導方針建置的 PAW 對於此角色已經足夠。<p>-   PAW 應該用於更新、修補執行 Windows Server、Linux 和其他作業系統的企業伺服器與應用程式，以及為其進行疑難排解的系統管理員。<br />-   可能需要新增 PAW 的專用管理工具，才能處理更大規模的這些系統管理員。|
|使用者工作站系統管理員 <br />- 第 2 層|是|使用階段 2 中提供的指導方針建置的 PAW 對於具有使用者裝置系統管理權限的角色 (例如技術服務人員和技術支援角色) 已經足夠。<p>-   PAW 上可能需要安裝其他應用程式，才能啟用票證管理和其他支援功能。<br />- 應在工作站上設定 Windows Defender 惡意探索防護。<br />    可能需要新增 PAW 的專用管理工具，才能處理更大規模的這些系統管理員。|
|SQL、SharePoint 或企業營運 (LOB) 系統管理員<br />- 第 1 層|是|使用階段 2 指導方針建置的 PAW 對於此角色已經足夠。<p>-   PAW 上可能需要安裝其他管理工具，才能允許系統管理員管理應用程式，而不需要使用遠端桌面連線到伺服器。|
|管理社交媒體顯示狀態的使用者|部分|使用階段 2 中提供的指導方針建置的 PAW 可以當做為這些角色提供安全性的起點使用。<p>-   使用 Azure Active Directory (AAD) 保護與管理社交媒體帳戶，以共用、保護並追蹤對於社交媒體帳戶的存取。<br />    如需有關這項功能的詳細資訊，請參閱[此部落格文章](/windows/security/identity-protection/credential-guard/credential-guard)。<br />-   輸出網路限制必須允許與這些服務的連線。 這可以透過允許開放式網際網路連線 (否定許多 PAW 保證的更高安全性風險)，或僅允許服務所需的 DNS 位址 (可能不容易取得) 完成。|
|標準使用者|否|雖然標準使用者可以使用許多強化步驟，但是 PAW 是針對隔離帳戶與多數使用者為了工作職責所需的開放式網際網路存取所設計。|
|客體 VDI/Kiosk|否|雖然客體的 Kiosk 系統可以使用許多強化步驟，但是 PAW 架構的設計是為了針對敏感度高的帳戶提供更高的安全性，而不是針對敏感度較低的帳戶提供更高的安全性。|
|VIP 使用者 (主管、研究人員等等)|部分|使用階段 2 中提供的指導方針建置的 PAW 可以當做為這些角色提供安全性的起點使用。<p>-   此案例類似於標準使用者桌面，但通常會有更小、更簡單，而且已知的應用程式設定檔。 此案例通常需要探索及保護機密資料、服務，以及 (不一定是安裝在桌上型電腦上的) 應用程式。<br />-   這些角色通常需要高度的安全性以及非常高度的可用性，這需要進行設計變更，才能符合使用者喜好設定。|
|工業控制系統 (例如 SCADA、PCN 和 DCS)|部分|使用階段 2 中提供的指導方針建置的 PAW 可以當做為這些角色提供安全性的起點使用，因為大部分的 ICS 主控台 (包括諸如 SCADA 和 PCN 這類的常見標準) 不需要瀏覽開放式網際網路，也不需要檢查電子郵件。<p>-   用於控制實體機器的應用程式必須經過整合和相容性測試，才能受到適當的保護。|
|內嵌的作業系統|否|雖然 PAW 的許多強化步驟可以用於內嵌的作業系統，但是在此案例中，為了強化，必須開發自訂解決方案。|

> [!NOTE]
> **組合案例**：有些人員可能會有跨越多個案例的管理責任。
> 在這些情況下，要牢記在心的重要規則是，必須一律遵循階層模型規則。 如需詳細資訊，請參閱階層模型頁面。

> [!NOTE]
> **擴充 PAW 程式**：由於您的 PAW 程式擴充為包含更多系統管理員及角色，因此您必須繼續確保您堅持遵守安全性標準及可用性。 這可能會要求您更新您的 IT 支援結構或建立新的 IT 支援結構，以解決 PAW 專屬的挑戰，例如 PAW 入門訓練程序、事件管理、設定管理，以及收集意見反應以因應可用性挑戰。  其中一個範例可能是您的組織決定為系統管理員啟用在家工作案例，這需要從桌上型電腦 PAW 轉變為筆記型電腦 PAW，這種轉變可能需要額外的安全性考量。  另一個常見的範例是為新的系統管理員建立或更新訓練，這個訓練現在必須包含正確使用 PAW 的內容 (包括這為什麼很重要，以及什麼是 PAW 和什麼不是 PAW)。  如需有關擴充 PAW 程式時必須處理的詳細考量，請參閱指示的階段 2。

本指導方針包含上述案例的 PAW 設定詳細指示。 如果您有其他案例的需求，可以根據本指導方針自行調整指示，或雇用一個專業的服務組織協助調整，例如 Microsoft。

如需有關吸引人的 Microsoft 服務詳細資訊，以便設計為您的環境量身打造的 PAW，請連絡您的 Microsoft 代表或瀏覽[本頁](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx)。

## <a name="paw-phased-implementation"></a>PAW 階段式實作

PAW 必須為系統管理提供安全且受信任的來源，建置程序安全且受信任相當重要。  本節將提供詳細的指示，這些指示可讓您使用非常類似 Microsoft IT 和 Microsoft 雲端工程與服務管理組織所使用的一般原則和概念，建置您自己的 PAW。

這些指示分為三個階段，其重點在於使最重要的緩和措施快速到位，然後為企業逐漸增加並擴展 PAW 的用途。

* [階段 1 - 立即部署 Active Directory 系統管理員](#phase-1-immediate-deployment-for-active-directory-administrators)
* [階段 2 - 將 PAW 延伸至所有系統管理員](#phase-2-extend-paw-to-all-administrators)
* [階段 3 - 進階的 PAW 安全性](#phase-3-extend-and-enhance-protection)

請務必注意，即使這些階段是以相同的整體專案部分加以規劃和實作，但是務必要依序執行。

### <a name="phase-1-immediate-deployment-for-active-directory-administrators"></a>階段 1：立即部署 Active Directory 系統管理員

目的：快速提供一個可以保護內部部署網域和樹系系統管理角色的 PAW。

範圍：第 0 層系統管理員，包括 Enterprise Admins、Domain Admins (適用於所有網域)，以及其他授權身份識別系統的系統管理員。

階段 1 著重在管理內部部署 Active Directory 網域的系統管理員，這些系統管理員是經常成為攻擊目標的非常重要角色。 不論您的 Active Directory 網域控制站 (DC) 是在內部部署資料中心、Azure 基礎結構即服務 (IaaS) 還是其他 IaaS 提供者中託管，這些身分識別系統都會有效率地運作，以保護這些系統管理員。

在此階段，您將建立安全的系統管理 Active Directory 組織單位 (OU) 結構，以託管您的特殊權限存取工作站 (PAW)，並部署 PAW 本身。  此結構也包含支援 PAW 所需的群組原則和群組。  您將使用 [TechNet 資源庫](https://aka.ms/pawmedia)中的 PowerShell 指令碼建立大部分的結構。

這些指令碼將會建立下列 OU 和安全性群組︰

* 組織單位 (OU)
   * 六個新的最上層 OU：系統管理員、群組、第 1 層伺服器、工作站、使用者帳戶、電腦隔離。  每個最上層 OU 都將包含一些子 OU。
* 群組
   * 六個已啟用安全性的新全域群組：第 0 層複寫維護、第 1 層伺服器維護、服務台操作員、工作站維護、PAW 使用者、PAW 維護。

您也會建立數個群組原則物件：PAW 設定 - 電腦；PAW 設定 - 使用者；必要的 RestrictedAdmin - 電腦；PAW 輸出限制；限制工作站登入；限制伺服器登入。

階段 1 包含下列步驟：

#### <a name="complete-the-prerequisites"></a>完成必要條件

1. **請確定所有系統管理員都會系統管理和使用者活動使用不同的個別帳戶** (包括電子郵件、網際網路瀏覽、企業營運應用程式，以及其他非系統管理活動)。  將系統管理帳戶指派給與其標準使用者帳戶不同的每個獲授權人員對於 PAW 模型而言相當重要，因為只有特定帳戶才能登入 PAW 本身。

   > [!NOTE]
   > 每個系統管理員都應該使用自己的帳戶進行系統管理。  請不要共用系統管理帳戶。

2. **將第 0 層特殊權限系統管理員的人數降至最低**。  由於每個系統管理員都必須使用 PAW，因此，減少系統管理員的人數就會減少支援他們所需的 PAW 數目，以及相關的成本。 系統管理員人數較低，也會使這些權限的曝光率和相關的風險較低。 雖然系統管理員可以在一個位置共用 PAW，但是不同實體位置的系統管理員需要個別的 PAW。
3. **向受信任的供應商取得符合所有技術需求的硬體**。 Microsoft 建議取得符合[使用 Credential Guard 保護網域認證](/windows/security/identity-protection/credential-guard/credential-guard)一文中技術需求的硬體。

   > [!NOTE]
   > 在不含這些功能的硬體上安裝的 PAW 可以提供重要的防護，但是將無法使用進階的安全性功能，例如 Credential Guard 和 Device Guard。  階段 1 部署不需要有 Credential Guard 和 Device Guard，但是強烈建議在階段 3 (進階強化) 時使用。
   >
   > 請確定用於 PAW 的硬體來自其安全性作法受到組織信任的製造商和供應商。 這是將乾淨來源準則應用在供應鏈安全上。
   >
   > 如需有關供應鏈安全重要性的詳細背景，請瀏覽[此網站](https://www.microsoft.com/security/cybersecurity/)。

4. **取得並驗證所需的 Windows 10 企業版和應用程式軟體。** 取得 PAW 所需的軟體，並使用[適用於安裝媒體的乾淨來源](https://aka.ms/cleansource)中的指導方針進行驗證。

   * Windows 10 企業版
   * Windows 10 的[遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)
   * [Windows 10 安全性基準](https://aka.ms/win10baselines)

      > [!NOTE]
      > Microsoft 會針對 MSDN 上的所有作業系統和應用程式發行 MD5 雜湊，但並非所有軟體廠商都提供類似的文件。  在這些情況下，將會需要其他策略。  如需有關驗證軟體的其他資訊，請參閱[適用於安裝媒體的乾淨來源](https://aka.ms/cleansource)。

5. **請確定您已在內部網路上使用 WSUS 伺服器**。 在內部網路上需要有 WSUS 伺服器，才能下載並安裝 PAW 的更新。 此 WSUS 伺服器應該設定為自動核准 Windows 10 適用的所有安全性更新，或者系統管理人員應該負責快速核准軟體更新。

   > [!NOTE]
   > 如需詳細資訊，請參閱[核准更新指導方針](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc708458(v=ws.10))中的「自動核准更新以供安裝」一節。

#### <a name="deploy-the-admin-ou-framework-to-host-the-paws"></a>部署系統管理 OU 架構以託管 PAW

1. 從 [TechNet 資源庫](https://aka.ms/PAWmedia)下載 PAW 指令碼程式庫

   > [!NOTE]
   > 下載所有檔案並儲存至相同的目錄，然後以下列指定的順序加以執行。  Create-PAWGroups 相依於 Create-PAWOUs 所建立的 OU 結構，而 Set-PAWOUDelegation 則相依於 Create-PAWGroups 所建立的群組。
   > 請勿修改任何指令碼或逗號分隔值 (CSV) 檔案。

2. **執行 Create-PAWOUs.ps1 指令碼**。  此指令碼將會在 Active Directory 中建立新的組織單位 (OU) 結構，並視需要，在新的 OU 上禁止繼承 GPO。
3. **執行 Create-PAWGroups.ps1 指令碼**。  此指令碼將會在適當的 OU 中，建立新的全域安全性群組。

   > [!NOTE]
   > 雖然這個指令碼會建立新的安全性群組，但是不會自動填入這些安全性群組。

4. **執行 Set-PAWOUDelegation.ps1 指令碼**。  此指令碼會將新 OU 的權限指派給適當的群組。

#### <a name="move-tier-0-accounts-to-the-admintier-0accounts-ou"></a>將第 0 層帳戶移至 Admin\Tier 0\Accounts OU。

將屬於 Domain Admin、Enterprise Admin 或第 0 層對等群組成員 (包括巢狀成員資格) 的每個帳戶移動至此 OU。 如果您的組織有您自己已新增至這些群組的群組，則應該將這些群組移至 Admin\Tier 0\Groups OU。

   > [!NOTE]
   > 如需其群組為第 0 層的詳細資訊，請參閱[保護特殊權限存取的參考資料](../securing-privileged-access/securing-privileged-access-reference-material.md)中的「第 0 層對應項」。

#### <a name="add-the-appropriate-members-to-the-relevant-groups"></a>將適當的成員新增至相關的群組

1. **PAW 使用者** - 新增第 0 層系統管理員，這些系統管理員擁有您在步驟 1 的階段 1 所識別的 Domain 或 Enterprise Admin 群組。
2. **PAW 爪子維護** - 新增至少一個用於 PAW 維護和疑難排解工作的帳戶。 只有在罕見的情況下，才會使用 PAW 維護帳戶。

   > [!NOTE]
   > 請不要將相同的使用者帳戶或群組同時新增至 PAW 使用者和 PAW 維護。  PAW 安全性模型部分是以假設 PAW 使用者帳戶對於受管理系統或 PAW 本身 (非兩者) 擁有特殊權限為基礎。
   >
   > * 這對於在階段 1 中建置良好的系統管理作法和習慣很重要。
   > * 這對於在階段 2 以後防止透過 PAW 將權限提升為要跨越階層的 PAW 也非常重要。
   >
   > 在理想情況下，不會在多個階層為任何人員指派職責，以強制執行職責劃分原則，但是 Microsoft 認為許多組織都可能會限制不允許這種完全劃分的人員 (或其他組織需求)。 在這些情況下，相同的人員可能會獲指派兩個角色，但不應該將相同的帳戶用於這些職責。

#### <a name="create-paw-configuration---computer-group-policy-object-gpo"></a>建立「PAW 設定 - 電腦」群組原則物件 (GPO)

在本節中，您將建立一個新的「PAW 設定 - 電腦」GPO (可為這些 PAW 提供特定的防護)，然後將此 GPO 連結到第 0 層裝置 OU (Tier 0\Admin 底下的裝置)。

   > [!NOTE]
   > **請不要將這些設定新增至預設網域原則**。  如此一來，可能會影響整個 Active Directory 環境的運作。  只有在此處所描述的新建立的 GPO 中進行這些設定，而且僅將這些設定套用至 PAW OU。

1. **PAW 維護存取** - 此設定會將 PAW 上特定權限群組的成員資格設定為一組特定的使用者。 移至 [電腦設定\喜好設定\控制台設定\本機使用者和群組]  ，並遵循下列步驟進行︰
   1. 按一下 [新增]  ，然後按一下 [本機群組]
   2. 選取 [更新]  動作，然後選取 [Administrators (內建)] (請不要使用 [瀏覽] 按鈕選取 Administrators 網域群組)。
   3. 選取 [刪除所有成員使用者]  和 [刪除所有成員群組]  核取方塊
   4. 新增 PAW 維護 (pawmaint) 和系統管理員 (同樣地，請不要使用 [瀏覽] 按鈕選取 Administrator)。

      > [!NOTE]
      > 請不要將 PAW Users 群組新增至本機 Administrators 群組的成員資格清單中。  為確保 PAW 使用者無法偶然或刻意修改 PAW 本身的安全性設定，他們不應該是本機 Administrators 群組的成員。
      >
      > 如需有關如何使用群組原則喜好設定修改群組成員資格的詳細資訊，請參閱 TechNet 文章[設定本機群組項目](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732525(v=ws.11))。

2. **限制本機群組成員資格** - 此設定將確保工作站上的本機系統管理員群組成員資格永遠是空白的
   1. 移至 [電腦設定\喜好設定\控制台設定\本機使用者和群組]，並遵循下列步驟進行︰
      1. 按一下 [新增]  ，然後按一下 [本機群組]
      2. 選取 [更新]  動作，然後選取 [Backup Operators (內建)] (請不要使用 [瀏覽] 按鈕選取 Backup Operators 網域群組)。
      3. 選取 [刪除所有成員使用者]  和 [刪除所有成員群組]  核取方塊。
      4. 請不要將任何成員新增至群組。  只要按一下 [確定]  。  群組原則將會透過指派空的清單，自動移除所有成員，並確保每次重新整理群組原則時，成員資格清單都是空白的。
   2. 針對以下其他群組，完成上述步驟︰
      * Cryptographic Operators
      * Hyper-V 系統管理員
      * Network Configuration Operators
      * Power Users
      * Remote Desktop Users
      * 複寫者
   3. **PAW 登入限制** - 此設定將會限制可以登入 PAW 的帳戶。 請遵循下列步驟進行此設定︰
      1. 移至 [電腦設定\原則\Windows 設定\安全性設定\本機原則\使用者權限指派\允許本機登入]。
      2. 選取 [定義這些原則設定]，然後新增 PAW 使用者和 Administrators (同樣地，請不要使用 [瀏覽] 按鈕來選取 Administrator)。
   4. **封鎖輸入網路流量** - 此設定將確保 PAW 不允許任何未經同意的輸入網路流量。 請遵循下列步驟進行此設定︰
      1. 移至 [電腦設定\原則\Windows 設定\安全性設定\具有進階安全性的 Windows 防火牆\具有進階安全性的 Windows 防火牆]，並遵循下列步驟進行︰
         1. 以滑鼠右鍵按一下 [具有進階安全性的 Windows 防火牆]，然後選取 [匯入原則]  。
         2. 按一下 [是]  ，接受這將會覆寫任何現有的防火牆原則。
         3. 瀏覽至 PAWFirewall.wfw，然後選取 [開啟]  。
         4. 按一下 [確定]  。

            > [!NOTE]
            > 您可以新增位址或子網路，而且這些位址或子網路目前必須到達含有未經同意流量的 PAW (例如，安全性掃描或管理軟體)。
            > WFW 檔案中的設定將會以「封鎖 - 預設」模式，啟用所有防火牆設定檔的防火牆、關閉規則合併，以及啟用已捨棄封包和成功封包的記錄。 這些設定將會封鎖未經同意的流量同時，同時仍然針對起始自 PAW 的連線，允許雙向通訊；防止擁有本機系統管理存取權的使用者建立將會覆寫 GPO 設定的本機防火牆規則；以及確保將進出 PAW 的流量記錄下來。
            > **開啟此防火牆將會擴大 PAW 的受攻擊面，並增加安全性風險。新增任何位址之前，請參閱本指導方針中的「管理和操作 PAW」一節**。

   5. **設定適用於 WSUS 的 Windows Update** - 請遵循下列步驟變更設定，以設定適用於 PAW 的 Windows Update︰
      1. 移至 [電腦設定\原則\系統管理範本\Windows 元件\Windows Update]，並遵循下列步驟進行︰
         1. 啟用 [設定自動更新]  原則。
         2. 選取 [4 - 自動下載和排程安裝]  選項。
         3. 將 [排程安裝日期]  選項變更為 [0 - 每天]  ，並將 [排程安裝時間]  選項變更為您組織的喜好設定。
         4. 啟用 [指定內部網路 Microsoft 更新服務的位置]  原則選項，並在這兩個選項中指定 ESAE WSUS 伺服器的 URL。
   6. 連結「PAW 設定 - 電腦」GPO，如下所示︰

         |原則|連結位置|
         |-----|---------|
         |PAW 設定 - 電腦 |Admin\Tier 0\Devices|

#### <a name="create-paw-configuration---user-group-policy-object-gpo"></a>建立「PAW 設定 - 使用者」群組原則物件 (GPO)

在本節中，您將建立一個新的「PAW 設定 - 使用者」GPO (可為這些 PAW 提供特定的防護)，然後將此 GPO 連結到第 0 層帳戶 OU (Tier 0\Admin 底下的帳戶)。

   > [!NOTE]
   > 請不要將這些設定新增至預設網域原則

1. **封鎖網際網路瀏覽** - 若要防止不小心瀏覽網際網路，這將會設定迴路位址 (127.0.0.1) 的 Proxy 位址。
   1. 移至 [使用者設定\喜好設定\Windows 設定\登錄]。 在 [登錄] 上按一下滑鼠右鍵，選取 [新增]   > [登錄項目]  ，並進行下列設定︰
      1. 動作：取代
      2. Hive：HKEY_CURRENT_USER
      3. 機碼路徑：Software\Microsoft\Windows\CurrentVersion\Internet Settings
      4. 值名稱：ProxyEnable

         > [!NOTE]
         > 請不要選取 [數值名稱] 左側的 [預設] 方塊。

      5. 值類型：REG_DWORD
      6. 數值資料：1
         1. 按一下 [通用] 索引標籤，然後選取 [當不再套用這個項目時移除它]  。
         2. 在 [通用] 索引標籤上，選取 [項目等級目標]  ，然後按一下 [目標]  。
         3. 按一下 [新增項目]  ，然後選取 [安全性群組]  。
         4. 選取 [...] 按鈕並瀏覽 PAW Users 群組。
         5. 按一下 [新增項目]  ，然後選取 [安全性群組]  。
         6. 選取 [...] 按鈕並瀏覽 **Cloud Services Admins** 群組。
         7. 按一下 [Cloud Services Admins]  項目，然後按一下 [項目選項]  。
         8. 選取 [不是]  。
         9. 按一下目標視窗上的 [確定]  。
      7. 按一下 [確定]  ，完成 ProxyServer 群組原則設定
   2. 移至 [使用者設定\喜好設定\Windows 設定\登錄]。 在 [登錄] 上按一下滑鼠右鍵，選取 [新增]   > [登錄項目]  ，並進行下列設定︰

      * 動作：取代
      * Hive：HKEY_CURRENT_USER
      * 機碼路徑：Software\Microsoft\Windows\CurrentVersion\Internet Settings
         * 值名稱：ProxyServer

            > [!NOTE]
            > 請不要選取 [數值名稱] 左側的 [預設] 方塊。

         * 值類型：REG_SZ
         * 數值資料：127.0.0.1:80
            1. 按一下 [通用]  索引標籤，然後選取 [當不再套用這個項目時移除它]  。
            2. 在 [通用]  索引標籤上，選取 [項目等級目標]  ，然後按一下 [目標]  。
            3. 按一下 [新增項目]  ，然後選取安全性群組。
            4. 選取 [...] 按鈕並新增 PAW Users 群組。
            5. 按一下 [新增項目]  ，然後選取安全性群組。
            6. 選取 [...] 按鈕並瀏覽 **Cloud Services Admins** 群組。
            7. 按一下 [Cloud Services Admins]  項目，然後按一下 [項目選項]  。
            8. 選取 [不是]  。
            9. 按一下目標視窗上的 [確定]  。

   3. 按一下 [確定]  ，完成 ProxyServer 群組原則設定。
2. 移至 [使用者設定\原則\系統管理範本\Windows 元件\Internet Explorer]，並啟用下列選項。 這些設定將會防止系統管理員手動覆寫 Proxy 設定。
   1. 啟用 [停用變更自動設定]  設定。
   2. 啟用 [防止變更 Proxy 設定]  。

#### <a name="restrict-administrators-from-logging-onto-lower-tier-hosts"></a>限制系統管理員登入較低層的主機

在本節中，我們將設定群組原則，以防特殊權限系統管理帳戶登入較低層的主機。

1. 建立新的**限制工作站登入** GPO - 此設定將會限制第 0 層和第 1 層系統管理員帳戶登入標準工作站。  此 GPO 應連結至「工作站」頂層 OU，並具有下列設定：

   * 在 [電腦設定\原則\Windows 設定\安全性設定\本機原則\使用者權限指派\拒絕以批次工作登入] 中，選取 [定義這些原則設定]  ，然後新增第 0 層和第 1 層群組︰

      ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > 內建第 0 層群組，如需詳細資訊，請參閱第 0 層對應項。

      其他委派的群組

     > [!NOTE]
     > 擁有有效第 0 層存取權的任何自訂建立群組，如需詳細資訊，請參閱第 0 層對應項。

      第 1 層系統管理員

     > [!NOTE]
     > 此群組是在階段 1 稍早建立。

   * 在 [電腦設定\原則\Windows 設定\安全性設定\本機原則\使用者權限指派\拒絕以服務方式登入] 中，選取 [定義這些原則設定]  ，然後新增第 0 層和第 1 層群組︰
      ```
      Enterprise Admins
      Domain Admins
      Schema Admins
      BUILTIN\Administrators
      Account Operators
      Backup Operators
      Print Operators
      Server Operators
      Domain Controllers
      Read-Only Domain Controllers
      Group Policy Creators Owners
      Cryptographic Operators
      ```

     > [!NOTE]
     > 注意：內建第 0 層群組，如需詳細資訊，請參閱第 0 層對應項。

      其他委派的群組

     > [!NOTE]
     > 注意：擁有有效第 0 層存取權的任何自訂建立群組，如需詳細資訊，請參閱第 0 層對應項。

      第 1 層系統管理員

     > [!NOTE]
     > 注意：此群組是在階段 1 稍早建立。

2. 建立新的**限制伺服器登入** GPO - 此設定將會限制第 0 層系統管理員帳戶登入第 1 層伺服器。  此 GPO 應連結至「第 1 層伺服器」頂層 OU，並具有下列設定：

   * 在 [電腦設定\原則\Windows 設定\安全性設定\本機原則\使用者權限指派\拒絕以批次工作登入] 中，選取 [定義這些原則設定]  ，然後新增第 0 層群組︰

      ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > 內建第 0 層群組，如需詳細資訊，請參閱第 0 層對應項。

      其他委派的群組

     > [!NOTE]
     > 擁有有效第 0 層存取權的任何自訂建立群組，如需詳細資訊，請參閱第 0 層對應項。

   * 在 [電腦設定\原則\Windows 設定\安全性設定\本機原則\使用者權限指派\拒絕以服務方式登入] 中，選取 [定義這些原則設定]  ，然後新增第 0 層群組︰

      ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
      ```

     > [!NOTE]
     > 內建第 0 層群組，如需詳細資訊，請參閱第 0 層對應項。

      其他委派的群組

     > [!NOTE]
     > 擁有有效第 0 層存取權的任何自訂建立群組，如需詳細資訊，請參閱第 0 層對應項。

   * 在 [電腦設定\原則\Windows 設定\安全性設定\本機原則\使用者權限指派\拒絕本機登入] 中，選取 [定義這些原則設定]  ，然後新增第 0 層群組︰

     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > 注意：內建第 0 層群組，如需詳細資訊，請參閱第 0 層對應項。

      其他委派的群組

     > [!NOTE]
     > 注意：擁有有效第 0 層存取權的任何自訂建立群組，如需詳細資訊，請參閱第 0 層對應項。

#### <a name="deploy-your-paws"></a>部署 PAW

   > [!NOTE]
   > 請確定在作業系統建置過程中，PAW 與網路中斷連線。

1. 使用您稍早取得的乾淨來源安裝媒體，安裝 Windows 10。

   > [!NOTE]
   > 您可以使用 Microsoft Deployment Toolkit (MDT) 或其他自動化的映像部署系統，將 PAW 部署自動化，但是您必須確定建置程序與 PAW 一樣值得信任。 敵手會專門找出公司映像和部署系統 (包括 ISO、部署套件等) 做為持續性機制，因此不應使用預先存在的部署系統或映像。
   >
   > 如果您將 PAW 的部署自動化，您必須︰
   >
   > * 使用透過[適用於安裝媒體的乾淨來源](https://aka.ms/cleansource)中的指導方針驗證的安裝媒體，建置系統。
   > * 請確定在作業系統建置過程中，自動化的部署系統與網路中斷連線。

2. 為本機 Administrator 帳戶設定一個複雜的唯一密碼。  請不要使用曾經用於環境中其他任何帳戶的密碼。

   > [!NOTE]
   > Microsoft 建議使用[本機系統管理員密碼解決方案 (LAPS)](https://www.microsoft.com/download/details.aspx?id=46899) 管理所有工作站的本機系統管理員密碼，包括 PAW。  如果您使用 LAPS，請確定您只授與 PAW Maintenance 群組權限，為 PAW 讀取受 LAPS 管理的密碼。

3. 使用乾淨來源安裝媒體，安裝 Windows 10 的遠端伺服器管理工具。
4. 設定 Windows Defender 惡意探索防護

   > [!NOTE]
   > 設定指導方針以供遵循

5. 將 PAW 連線至網路。  請確定 PAW 可以連線到至少一個網域控制站 (DC)。
6. 使用 PAW Maintenance 群組成員的帳戶，從新建立的 PAW 執行下列 PowerShell 命令，以便將其加入至適當 OU 的網域中︰

   `Add-Computer -DomainName Fabrikam -OUPath "OU=Devices,OU=Tier 0,OU=Admin,DC=fabrikam,DC=com"`

   如果適當，將 *Fabrikam* 的參照取代成您的網域名稱。  如果您的網域名稱延伸到多個層級 (例如 child.fabrikam.com)，請依照網域的完整網域名稱中出現這些名稱的順序，新增具有 "DC =" 識別碼的其他名稱。

   > [!NOTE]
   > 如果您已經部署 [ESAE 系統管理樹系](https://aka.ms/esae) (適用於階段 1 的第 0 層系統管理員) 或 [Microsoft Identity Manager (MIM) 特殊權限存取管理 (PAM)](https://aka.ms/mimpamdeploy) (適用於階段 2 的第 1 層和第 2 層系統管理員)，則您會在這裡，將 PAW 加入至該環境中的網域，而不是實際執行的網域。

7. 請先套用所有重大及重要的 Windows Update，然後再安裝其他任何軟體 (包括系統管理工具、代理程式等)。
8. 強制執行群組原則應用程式。
   1. 開啟提升權限的命令提示字元，然後輸入下列命令：`Gpupdate /force /sync`
   2. 重新啟動電腦

9. (選擇性) 安裝 Active Directory 系統管理員所需的其他工具。 安裝執行工作職責所需的其他任何工具或指令碼。 請務必先使用任何工具，在目標電腦上評估認證暴露的風險，然後再將其新增至 PAW。 存取[本頁](https://aka.ms/logontypes)可取得有關評估系統管理工具及連線方法是否存在認證暴露風險的詳細資訊。 請務必使用[適用於安裝媒體的乾淨來源](https://aka.ms/cleansource)中的指導方針，取得所有安裝媒體。

   > [!NOTE]
   > 即使跳躍伺服器並不是當做安全性界限，在這些工具的中央位置使用跳躍伺服器還是可以降低複雜性。

10. (選擇性) 下載並安裝所需的遠端存取軟體。 如果系統管理員將要從遠端使用 PAW 進行系統管理，請使用遠端存取解決方案廠商所提供的安全性指導方針安裝遠端存取軟體。 請務必使用適用於安裝媒體的乾淨來源中的指導方針，取得所有安裝媒體。

    > [!NOTE]
    > 請仔細考慮與允許透過 PAW 進行遠端存取相關的所有風險。  雖然行動 PAW 可實現許多重要的案例 (包括在家工作)，但是遠端存取軟體可能容易受到攻擊，而且可用來危害 PAW。

11. 使用下列步驟，檢閱並確認所有設定都正確無誤，藉此驗證 PAW 系統的完整性︰
    1. 確認只有 PAW 專屬的群組原則才會套用至 PAW
       1. 開啟提升權限的命令提示字元，然後輸入下列命令：`Gpresult /scope computer /r`
       2. 檢閱所產生的清單，並確定只有出現的群組原則是您先前建立的群組原則。
    2. 使用下列步驟，確認 PAW 上沒有其他使用者帳戶是特殊權限群組的成員︰
       1. 開啟 [編輯本機使用者和群組]  (lusrmgr.msc)，選取 [群組]  ，然後確認只有的本機 Administrators 群組的成員才是本機 Administrator 帳戶和 PAW Maintenance 全域安全性群組。

          > [!NOTE]
          > PAW Users 群組應該不是本機 Administrators 群組的成員。  只有成員才應該是本機 Administrator 帳戶和 PAW Maintenance 全域安全性群組 (且 PAW Users 應該也不是該全域群組的成員)。

       2. 此外，請使用 [編輯本機使用者和群組]  確定下列群組沒有任何成員︰備份操作員、Cryptographic Operator、Hyper-V 系統管理員、Network Configuration Operator、進階使用者、遠端桌面使用者、複寫者

12. (選擇性) 如果您的組織使用安全性資訊及事件管理 (SIEM) 解決方案，請確認 PAW [設為使用 Windows 事件轉送 (WEF) 將事件轉送到系統](/archive/blogs/jepayne/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem)，或使用解決方案登錄，讓 SIEM 主動地接收來自 PAW 的事件和資訊。  此操作的詳細資料會因為您的 SIEM 解決方案而有所不同。

    > [!NOTE]
    > 如果您的 SIEM在 PAW 上需要有以系統或本機系統管理帳戶身分執行的代理程式，請確定 SIEM 是使用與您的網域控制站和身分識別系統相同的信任層級加以管理。

13. (選擇性) 如果您選擇在 PAW 上部署 LAPS 來管理本機 Administrator 帳戶的密碼，請確認已成功註冊密碼。

    * 使用具有權限的帳戶讀取受 LAPS 管理的密碼，開啟 **Active Directory 使用者和電腦** (dsa.msc)。  確定已啟用 [進階功能]，然後以滑鼠右鍵按一下適當的電腦物件。  選取 [屬性編輯器] 索引標籤，並確認 msSVSadmPwd 的值以有效的密碼填入。

### <a name="phase-2-extend-paw-to-all-administrators"></a>階段 2：將 PAW 延伸至所有系統管理員

範圍：對於關鍵任務應用程式和相依項目擁有系統管理權限的所有使用者。  這應該至少包含應用程式伺服器、操作健全狀況與安全性監視解決方案、虛擬化解決方案、儲存系統，以及網路裝置的系統管理員。

> [!NOTE]
> 此階段中的指示會假設階段 1 已經全部完成。  在您完成階段 1 的所有步驟之前，請不要開始階段 2。

一旦您確認已完成所有步驟，請執行下列步驟以完成階段 2︰

#### <a name="recommended-enable-restrictedadmin-mode"></a>(建議) 啟用 **RestrictedAdmin** 模式

在您現有的伺服器和工作站上啟用此功能，然後強制使用此功能。 此功能會要求目標伺服器執行 Windows Server 2008 R2 或更新版本，並要求目標工作站執行 Windows 7 或更新版本。

1. 請遵循[本頁](https://aka.ms/rdpra)中提供的指示，在您的伺服器和工作站上啟用 **RestrictedAdmin** 模式。

   > [!NOTE]
   > 為網際網路對向伺服器啟用此功能之前，您應該考慮敵手能夠向密碼雜湊先前遭竊的這些伺服器進行驗證的風險。

2. 建立「需要 RestrictedAdmin - 電腦」群組原則物件 (GPO)。 本節將建立強制使用 /RestrictedAdmin 參數進行傳出遠端桌面連線的 GPO，以保護帳戶免於遭受目標系統上的認證竊取
   * 移至 [電腦設定\原則\系統管理範本\系統\認證委派\限制委派認證給遠端伺服器]，並設為 [啟用]  。
3. 使用下列原則選項，將「需要 **RestrictedAdmin** - 電腦」連結至適當的第 1 層和/或第 2 層裝置︰
   * PAW 設定 - 電腦
      * -> 連結位置：Admin\Tier 0\Devices (現有的)
   * PAW 設定 - 使用者
      * -> 連結位置：Admin\Tier 0\Accounts
   * 需要 RestrictedAdmin - 電腦
      * ->Admin\Tier1\Devices 或 -> Admin\Tier2\Devices (兩者都是選擇性的)

   > [!NOTE]
   > 對於第 0 層系統而言，這是不需要的，因為這些系統已在環境中完整控制所有資產。

#### <a name="move-tier-1-objects-to-the-appropriate-ous"></a>將第 1 層物件移至適當的 OU

1. 將第 1 層群組移至 Admin\Tier 1\Groups OU。 找出授與下列系統管理權限的所有群組，並將其移至此 OU。
   * 多部伺服器上的本機系統管理員
      * 雲端服務的系統管理存取權
      * 企業應用程式的系統管理存取權
2. 將第 1 層帳戶移至 Admin\Tier 1\Accounts OU。 將屬於那些第 1 層群組成員的每個帳戶 (包括巢狀成員資格) 移至此 OU。
3. 將適當的成員新增至相關的群組
   * **第 1 層系統管理員** - 此群組包含將受到限制而無法登入第 2 層主機的第 1 層系統管理員。 新增對於伺服器或網際網路服務具有系統管理權限的所有第 1 層系統管理群組。

      > [!NOTE]
      > 如果系統管理人員負責管理多個階層的資產，則您必須為每一層建立一個不同的系統管理帳戶。

4. 啟用 Credential Guard 可降低認證遭竊並重複使用的風險。  Credential Guard 是 Windows 10 的一個新功能，可限制應用程式對於認證的存取，以防止認證竊取攻擊 (包括傳遞雜湊)。  Credential Guard 對於使用者是完全透明的，而且需要的設定時間和精力最少。  如需有關 Credential Guard 的其他資訊，包括部署步驟和硬體需求，請參閱[使用 Credential Guard 保護網域認證](/windows/security/identity-protection/credential-guard/credential-guard)一文。

   > [!NOTE]
   > 若要設定和使用 Credential Guard，必須先啟用 Device Guard。  不過，您不需要設定其他任何 Device Guard 防護，就能使用 Credential Guard。

5. (選擇性) 啟用雲端服務的連線。 此步驟允許利用適當的安全性保證，管理雲端服務，例如 Azure 和 Office 365。 若要讓 Microsoft Intune 管理 PAW，也需要此步驟。

   > [!NOTE]
   > 如果雲端服務的系統管理或 Intune 的管理不需要有雲端連線，請略過此步驟。

   * 這些步驟會將網際網路上的通訊限制為僅限獲授權的雲端服務 (但不是開啟式網際網路)，並對將會處理網際網路內容的瀏覽器和其他應用程式增加防護。 用於系統管理的這些 PAW 永遠不會用於標準使用者工作，例如網際網路通訊和生產力。
   * 若要啟用 PAW 服務的連線，請遵循下列步驟︰

   1. 設定 PAW 僅允許獲授權的網際網路目的地。  當您擴充 PAW 部署以啟用雲端管理時，必須允許存取授權的服務，同時篩選出來自開啟式網際網路的存取，因為開啟式網際網路更容易對您的系統管理員發動攻擊。

      1. 建立 **Cloud Services Admins** 群組，並將網際網路上需要存取雲端服務的所有帳戶新增至該群組。
      2. 從 [TechNet 資源庫](https://aka.ms/pawmedia)下載 PAW *proxy.pac* 檔案，並在內部網站上發佈該檔案。

         > [!NOTE]
         > 您必須在下載後更新 *proxy.pac* 檔案，以確保這是最新而且完整的檔案。
         > Microsoft 會在 Office [支援中心](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)發佈所有最新的 Office 365 和 Azure URL。 這些指示假設您將使用 Internet Explorer (或 Microsoft Edge) 進行 Office 365、Azure 和其他雲端服務的系統管理作業。 Microsoft 建議為進行系統管理所需的任何協力廠商瀏覽器，設定類似的限制。 PAW 上的網頁瀏覽器應該僅用於雲端服務的系統管理，而且絕不會用於一般網頁瀏覽。
         >
         > 您可能需要新增其他有效的網際網路目的地，才能為其他 IaaS 提供者新增至此清單，但是請不要將生產力、娛樂、新聞，或搜尋網站新增至此清單。
         >
         > 您也可能需要調整 PAC 檔案以配合這些位址所使用的有效 Proxy 位址。
         >
         > 您也可以使用 Web Proxy 限制來自 PAW 的存取，以便進行深入防禦。 我們不建議在沒有 PAC 檔案的情況下自行使用此方式，因為這只會在連線至公司網路時，才限制對 PAW 的存取。

      3. 一旦您設定 *proxy.pac* 檔案之後，更新 PAW 設定 - 使用者 GPO。
         1. 移至 [使用者設定\喜好設定\Windows 設定\登錄]。 在 [登錄] 上按一下滑鼠右鍵，選取 [新增]   > [登錄項目]  ，並進行下列設定︰
            1. 動作：取代
            2. Hive：HKEY_CURRENT_USER
            3. 機碼路徑：Software\Microsoft\Windows\CurrentVersion\Internet Settings
            4. 值名稱：AutoConfigUrl

               > [!NOTE]
               > 請不要選取 [數值名稱] 左側的 [預設]  方塊。

            5. 值類型：REG_SZ
            6. 數值資料︰輸入 *proxy.pac* 檔案的完整 URL，包括 http:// 和檔案名稱，例如 http://proxy.fabrikam.com/proxy.pac 。  URL 也可以是單一標籤 URL，例如 http://proxy/proxy.pac 。

               > [!NOTE]
               > PAC 檔案也可以裝載在檔案共用上，其語法為 file://server.fabrikan.com/share/proxy.pac，但這必須允許使用 file:// 通訊協定。 如需有關設定所需登錄值的其他詳細資訊，請參閱這個[了解 Web Proxy 設定](https://blogs.msdn.com/b/ieinternals/archive/2013/10/11/web-proxy-configuration-and-ie11-changes.aspx)部落格的「附註：File:// 型 Proxy 指令碼已過時」一節。

            7. 按一下 [通用]  索引標籤，然後選取 [當不再套用這個項目時移除它]  。
            8. 在 [通用]  索引標籤上，選取 [項目等級目標]  ，然後按一下 [目標]  。
            9. 按一下 [新增項目]，然後選取 [安全性群組]。
            10. 選取 [...] 按鈕並瀏覽 **Cloud Services Admins** 群組。
            11. 按一下 [新增項目]  ，然後選取 [安全性群組]  。
            12. 選取 [...] 按鈕並瀏覽 **PAW Users** 群組。
            13. 按一下 [PAW 使用者]  項目，然後按一下 [項目選項]  。
            14. 選取 [不是]  。
            15. 按一下目標視窗上的 [確定]  。
            16. 按一下 [確定]  ，完成 **AutoConfigUrl** 群組原則設定。
   2. 套用 Windows 10 安全性基準和雲端服務存取。使用下列步驟，將 Windows 和雲端服務存取 (如果需要) 的安全性基準連結至正確的 OU︰
      1. 將 Windows 10 安全性基準 ZIP 檔案的內容解壓縮。
      2. 建立這些 GPO、[匯入原則](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753786(v=ws.11))設定，並依照此表格[連結](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732979(v=ws.11))。 將每個原則連結到每個位置，並確定遵循表格中的順序 (表格中比較下方的項目應該比較晚套用，且優先順序比較高)︰

         **原則：**

         | 原則名稱 | 連結 |
         |--|--|
         | CM Windows 10 - 網域安全性 | N/A - 現在不要連結 |
         | SCM Windows 10 TH2 - 電腦 | Admin\Tier 0\Devices |
         |  | Admin\Tier 1\Devices |
         |  | Admin\Tier 2\Devices |
         | SCM Windows 10 TH2- BitLocker | Admin\Tier 0\Devices |
         |  | Admin\Tier 1\Devices |
         |  | Admin\Tier 2\Devices |
         | SCM Windows 10 - Credential Guard | Admin\Tier 0\Devices |
         |  | Admin\Tier 1\Devices |
         |  | Admin\Tier 2\Devices |
         | SCM Internet Explorer - 電腦 | Admin\Tier 0\Devices |
         |  | Admin\Tier 1\Devices |
         |  | Admin\Tier 2\Devices |
         | PAW 設定 - 電腦 | Admin\Tier 0\Devices (現有的) |
         |  | Admin\Tier 1\Devices (新連結) |
         |  | Admin\Tier 2\Devices (新連結) |
         | 需要 RestrictedAdmin - 電腦 | Admin\Tier 0\Devices |
         |  | Admin\Tier 1\Devices |
         |  | Admin\Tier 2\Devices |
         | SCM Windows 10 - 使用者 | Admin\Tier 0\Devices |
         |  | Admin\Tier 1\Devices |
         |  | Admin\Tier 2\Devices |
         | SCM Internet Explorer - 使用者 | Admin\Tier 0\Devices |
         |  | Admin\Tier 1\Devices |
         |  | Admin\Tier 2\Devices |
         | PAW 設定 - 使用者 | Admin\Tier 0\Devices (現有的) |
         |  | Admin\Tier 1\Devices (新連結) |
         |  | Admin\Tier 2\Devices (新連結) |

         > [!NOTE]
         > 「SCM Windows 10 - 網域安全性」GPO 可能會連結到 PAW 之外的網域，但是將會影響整個網域。

6. (選擇性) 安裝第 1 層系統管理員所需的其他工具。 安裝執行工作職責所需的其他任何工具或指令碼。 請務必先使用任何工具，在目標電腦上評估認證暴露的風險，然後再將其新增至 PAW。 如需有關評估系統管理工具及連線方法是否存在認證暴露風險的詳細資訊，請瀏覽[本頁](https://aka.ms/logontypes)。 請務必使用適用於安裝媒體的乾淨來源中的指導方針，取得所有安裝媒體
7. 找出並安全地取得系統管理作業所需的軟體和應用程式。  這類似於階段 1 中執行的工作，但範圍更大，因為要保護的應用程式、服務和系統數目日益增加。

   > [!NOTE]
   > 請確定您選擇 Windows Defender 惡意探索防護所提供的防護來保護這些新的應用程式 (包括網頁瀏覽器)。

   其他軟體和應用程式的範例包括︰

      * Microsoft Azure PowerShell
      * Office 365 PowerShell (也稱為 Microsoft Online Services 模組)
      * 以 Microsoft Management Console 為基礎的應用程式或服務管理軟體
      * 專屬的 (非 MMC 架構) 應用程式或服務管理軟體

         > [!NOTE]
         > 許多應用程式現在都透過網頁瀏覽器專門管理，包括許多雲端服務。  雖然這可以減少在 PAW 上安裝所需的應用程式數目，但是這也引入了瀏覽器互通性問題的風險。  您可能需要將非 Microsoft 網頁瀏覽器部署到特定的 PAW 執行個體，才能啟用特定服務的系統管理。  如果您有部署其他網頁瀏覽器，請確認您有遵循所有乾淨來源準則，並根據廠商的安全性指導方針，保護瀏覽器的安全。

8. (選擇性) 下載並安裝所需的任何管理代理程式。

   > [!NOTE]
   > 如果您選擇安裝其他管理代理程式 (監視、安全性、設定管理等)，您必須確保管理系統在與網域控制站和身分識別系統相同的層級都受到信任。 如需其他指導方針，請參閱「管理與更新 PAW」。

9. 評估您的基礎結構，以找出需要 PAW 所提供的其他安全性防護的系統。  請確定您確實知道哪些系統必須受到保護。  詢問有關資源本身的重大問題，例如︰

   * 必須受到管理的目標系統在哪裡？  這些系統是集中在單一實體位置，還是連線至已明確定義的單一子網路？
   * 有多少個系統？
   * 這些系統是否相依於其他系統 (虛擬化、儲存體等)，若是如此，如何管理這些系統？  重要系統如何暴露在這些相依性風險之中？還有哪些與這些相依性相關聯的其他風險？
   * 服務受到管理有多重要？如果這些服務遭到入侵，預期有什麼損失？

      > [!NOTE]
      > 將您的雲端服務加入至此評估中。攻擊者漸漸地鎖定不安全的雲端部署做為目標，因此請務必以您對於內部部署關鍵任務應用程式同等安全的方式，管理這些服務。

        使用這項評估找出需要額外保護的特定系統，然後將您的 PAW 程式擴充到這些系統的系統管理員。  從 PAW 型系統管理獲益良多的常見系統範例包括 SQL Server (內部部署和 SQL Azure)、人力資源應用程式，以及財務軟體。

        > [!NOTE]
        > 如果資源是從 Windows 系統管理，則即使應用程式本身是在 Windows 以外的作業系統或非 Microsoft 雲端平台上執行，還是可以使用 PAW 加以管理。  例如，Amazon Web Services 訂閱的擁有者應該僅使用 PAW 管理該帳戶。

10. 開發一個申請發佈方法，以便在您的組織中大規模地部署 PAW。  根據您在階段 2 中選擇部署的 PAW 數目而定，您可能需要將程序自動化。

    * 請考慮開發一個正式的申請核准程序，讓系統管理員用來取得 PAW。  此程序有助於將部署程序標準化，可確保 PAW 裝置的權責，並協助找出 PAW 部署的缺失。
    * 如先前所述，此部署解決方案應該與現有的自動化方法 (這可能已遭到危害) 有所區隔，而且應該遵循階段 1 中所述的準則。

        > [!NOTE]
        > 管理資源的任何系統都應該以相同或更高的信任層級管理本身。

11. 檢閱並視需要部署其他 PAW 硬體設定檔。  您為階段 1 部署所選擇的硬體設定檔可能不適用於所有系統管理員。  檢閱硬體設定檔，並在適當時，選取其他 PAW 硬體設定檔，以符合系統管理員的需求。  例如，專用硬體設定檔 (區別 PAW 和日常使用的工作站) 可能不適用於經常出差的系統管理員，在此情況下，您可以選擇為該系統管理員部署同時使用設定檔 (具有使用者 VM 的 PAW)。
12. 請考慮伴隨擴充 PAW 部署而出現的文化、操作、通訊，以及訓練需求。   這種對於系統管理模型的重大變更自然需要在一定程度上變更管理，因此請務必將其建置到部署專案本身。  請至少考慮下列問題︰

    * 如何將變動傳達到公司高級領導階層，以確保他們支持這些變動？  沒有公司高級領導階層做為後盾的任何專案都很可能會失敗，最起碼得努力籌措資金以及使其廣為接受。
    * 如何為系統管理員記載新的程序？  這些變動必須加以記載，並傳達給現有的系統管理員 (必須改變他們的習慣，並以不同的方式管理資源)，以及新的系統管理員 (從組織內部晉升或從組織外部雇用)。  請務必要清楚記載並完整表達威脅的重要性、PAW 在保護系統管理員方面的角色，以及正確使用 PAW 的方式。

      > [!NOTE]
      > 這對於流動率高的角色特別重要，包括但不限於技術支援人員。

    * 如何確保符合新的程序？  雖然 PAW 模型包含一些技術上的控制，可防止暴露特殊權限認證，但是單純利用技術上的控制不可能完全避免所有可能的風險。  例如，雖然技術上的控制可以防止系統管理員使用特殊權限認證成功登入使用者桌面，但是嘗試登入這個簡單的動作可能會將認證洩漏給該使用者桌面上安裝的惡意程式碼。  因此，清楚表達 PAW 模型的優點以及不合規的風險相當重要。  這應該透過稽核和警示補強，如此便可以快速偵測到認證暴露並加以解決。

### <a name="phase-3-extend-and-enhance-protection"></a>階段 3：擴充及增強防護

範圍：這些防護可以透過以進階功能 (包括多重要素驗證與網路存取規則) 鞏固基本防護，藉此增強階段 1 中建置的系統。

> [!NOTE]
> 完成階段 1 之後，隨時都可以執行這個階段。  此階段不相依於階段 2 的完成，因此可以在階段 2 前後執行，或與階段 2 同時執行。

請遵循下列步驟設定此階段︰

1. **為特殊權限帳戶啟用多重要素驗證**。  多重要素驗證會要求使用者在提供認證之外，也提供一個實體權杖，藉以加強帳戶安全性。  多重要素驗證可以完美地補強驗證原則，但是該驗證不相依於部署的驗證原則 (同樣地，驗證原則也不相依於多重要素驗證)。  Microsoft 建議使用以下其中一種形式的多重要素驗證︰

   * **智慧卡**：智慧卡是一個防竄改的可攜式實體裝置，可以在 Windows 登入過程中提供另一個驗證。  您可以要求個人必須擁有智慧卡才能登入，藉此減少從遠端重複使用遭竊憑證的風險。  如需有關 Windows 中的智慧卡登入詳細資訊，請參閱[智慧卡概觀](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831433(v=ws.11))一文。
   * **虛擬智慧卡**：虛擬智慧卡可提供與實體智慧卡相同的安全性優點，另一個優點則是可以連結至特定的硬體。  如需有關部署和硬體需求的詳細資訊，請參閱[虛擬智慧卡概觀](/previous-versions/windows/it-pro/windows-8.1-and-8/dn593708(v=ws.11))和[開始使用虛擬智慧卡︰逐步解說指南](/previous-versions/windows/it-pro/windows-8.1-and-8/dn579260(v=ws.11))這兩篇文章。
   * **Windows Hello 企業版**：Windows Hello 企業版可讓使用者驗證 Microsoft 帳戶、Active Directory 帳戶、Microsoft Azure Active Directory (Azure AD) 帳戶，或支援 Fast ID Online (FIDO) 驗證的非 Microsoft 服務。 在 Windows Hello 企業版註冊期間的初始雙步驟驗證之後，Windows Hello 企業版就會在使用者的裝置上設定完成，而使用者可以設定手勢 (可以是 Windows Hello 或 PIN)。 Windows Hello 企業版認證是非對稱金鑰組，在信賴平台模組 (TPM) 的隔離環境內可以產生這個金鑰組。
      如需 Windows Hello 企業版的詳細資訊，請參閱 [Windows Hello 企業版](/windows/security/identity-protection/hello-for-business/hello-identity-verification)一文。
   * **Azure 多重要素驗證**：Azure 多重要素驗證 (MFA) 可提供另一個驗證要素的安全性，並透過監視和機器學習式分析，提供增強的防護。  Azure MFA 不僅可以保護 Azure 系統管理員的安全，還可以保護其他許多解決方案，包括 Web 應用程式、Azure Active Directory，以及內部部署解決方案 (例如遠端存取和遠端桌面)。  如需有關 Azure 多重要素驗證的詳細資訊，請參閱[多重要素驗證](https://azure.microsoft.com/services/multi-factor-authentication)一文。

2. **使用 Windows Defender 應用程式控制和/或 AppLocker，將受信任的應用程式加入允許清單**。  透過限制未受信任或未簽署的程式碼在 PAW 上執行的能力，您就可以進一步降低惡意活動及危害的可能性。  Windows 對於應用程式控制，包含兩個主要選項︰

   * **Applocker**：AppLocker 可協助系統管理員控制可以在指定系統上執行的應用程式。  AppLocker 可以透過群組原則集中加以控制，而且可以套用至特定的使用者或群組 (針對以 PAW 使用者為目標的應用)。  如需有關 AppLocker 的詳細資訊，請參閱 TechNet 文章 [AppLocker 概觀](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831440(v=ws.11))。
   * **Windows Defender 應用程式控制**︰新的 Windows Defender 應用程式控制功能可提供增強的硬體式應用程式控制項，與 AppLocker 不同的是，無法在受影響的裝置上覆寫該控制項。  如同 AppLocker，Windows Defender 應用程式控制可以透過群組原則加以控制，並以特定使用者為目標。  有關如何使用 Windows Defender 應用程式控制限制應用程式使用方式，詳細資訊請參閱 TechNet 文章 [Windows Defender 應用程式控制部署指南](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide)。

3. **使用 Protected Users、驗證原則和驗證定址接收器進一步保護特殊權限帳戶**。  Protected Users 的成員受制於其他安全性原則，這些原則可保護本機安全性代理程式 (LSA) 中所儲存的認證，並大幅降低認證竊取和重複使用的風險。  驗證原則和定址接收器可控制特殊權限使用者如何存取網域中的資源。  整體而言，這些防護可大幅增強這些特殊權限使用者的帳戶安全性。  如需有關這些功能的其他詳細資訊，請參閱[如何設定受保護的帳戶](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn518179(v=ws.11))網路文章。

   > [!NOTE]
   > 這些防護的用意在於補強，而不是取代階段 1 中現有的安全性措施。  系統管理員仍然應該將不同的帳戶用於系統管理和一般用途。

## <a name="managing-and-updating"></a>管理與更新

PAW 必須具備反惡意程式碼功能，而且必須快速套用軟體更新，以維持這些工作站的完整性。

其他設定管理、操作監控及安全性管理也可以搭配 PAW 使用，但是必須審慎考慮是否要整合這些功能，因為每個管理功能也會透過該工具，引入 PAW 危害的風險。 推出進階管理功能是否有意義，取決於幾個因素，包括︰

* 管理功能的安全性狀態和作法 (包括工具的軟體更新作法、系統管理角色及這些角色中的帳戶、裝載或管理工具所在的作業系統，以及該工具的其他任何硬體或軟體相依性)
* 軟體部署和更新在 PAW 上的頻率和數量
* 詳細清查和設定資訊的需求
* 安全性監視需求
* 組織的標準與其他組織特有的因素

根據乾淨來源準則，用於管理或監視 PAW 的所有工具都必須在 PAW 層級以上受到信任。 這通常需要從 PAW 管理這些工具，以確保沒有來自較低權限工作站的安全性相依性。

此表格列出管理和監視 PAW 可能使用的不同方法︰

|方法|考量|
|------|---------|
|PAW 中的預設值<p>-   Windows Server Update Services<br />-   Windows Defender|-   不需要額外成本<br />-   執行基本的必要安全性功能<br />-   隨附在本指導方針中的指示|
|使用 [Intune](/mem/intune/) 管理|<ul><li>提供雲端式的可見度及控制<p><ul><li>軟體部署</li><li>o   管理軟體更新</li><li>Windows 防火牆原則管理</li><li>反惡意程式碼防護</li><li>遠端協助</li><li>軟體授權管理。</li></ul></li><li>不需要伺服器基礎結構</li><li>需要遵循階段 2 中的「啟用雲端服務的連線」步驟</li><li>如果 PAW 電腦未加入網域，這需要使用安全性基準下載所提供的工具，將 SCM 基準套用至本機映像。</li></ul>|
|用於管理 PAW 的新 System Center 執行個體|-   提供設定、軟體部署和安全性更新的可見度及控制<br />-   需要不同的伺服器基礎結構，將其固定至 PAW 的層級，並為這些高特殊權限的人員提供專業人員的技能|
|使用現有的管理工具管理 PAW|-   除非將現有的管理基礎結構培養到 PAW 的安全性層級，否則會對 PAW 的危害造成相當重大的風險 **附註︰**   除非您的組織有使用這種方法的特定原因，否則 Microsoft 通常不建議這麼做。 根據我們的經驗，將所有這些工具 (及其安全性相依項目) 都培養到 PAW 的安全性層級，通常需要非常高的成本。<br />-   其中大部分的工具都提供設定、軟體部署和安全性更新的可見度及控制|
|需要系統管理員存取權的安全性掃描或監視工具|包含可安裝代理程式的任何工具，或需要具有本機系統管理存取權的帳戶。<p>-   需要將工具安全保證培養到 PAW 的層級。<br />-   可能需要降低 PAW 的安全性情勢，才能支援工具功能 (開放連接埠、安裝 Java 或其他中介軟體等)，進而產生安全性取捨決策，|
|安全性資訊及事件管理 (SIEM)|<ul><li>如果 SIEM 是無代理程式<p><ul><li>不需要系統管理存取權，就可以使用 **Event Log Readers** 群組中的帳戶，存取 PAW 上的事件</li><li>需要開放網路連接埠，以允許來自 SIEM 伺服器的輸入流量</li></ul></li><li>如果 SIEM 需要一個代理程式，請參閱其他列**需要系統管理員存取權的安全性掃描或監視工具**。</li></ul>|
|Windows 事件轉送|-   提供一種無代理程式方法，將安全性事件從 PAW 轉送至外部收集器或 SIEM<br />-   不需要系統管理存取權，就可以存取 PAW 上的事件<br />-   不需要開放網路連接埠，就可以允許來自 SIEM 伺服器的輸入流量|

## <a name="operating-paws"></a>操作 PAW

PAW 解決方案應該根據乾淨來源準則，使用[操作標準](https://aka.ms/securitystandards)中的標準進行操作。

## <a name="deploy-paws-using-a-guarded-fabric"></a>使用受防護網狀架構部署 PAW

[受防護網狀架構](https://aka.ms/shieldedvms)可用來在膝上型電腦或跳躍伺服器上的受防護虛擬機器中執行 PAW 工作負載。
採用這種方法需要額外的基礎結構和操作步驟，但可讓您更輕鬆地定期重新部署 PAW 映像，並讓您將多個不同層 (或分類) 的 PAW 合併到在單一裝置上並存執行的虛擬機器。
如需有關受防護網狀架構拓撲和安全性承諾的完整說明，請參閱[受防護網狀架構文件](https://aka.ms/shieldedvms)。

### <a name="changes-to-the-paw-gpos"></a>PAW GPO 的變更

使用受防護 VM 架構下的 PAW 時，您必須修改上面定義的[建議的 GPO 設定](#create-paw-configuration---computer-group-policy-object-gpo)，以支援虛擬機器的使用。

1. 為實體 PAW 主機建立新的 OU。 實體和虛擬 PAW 有不同的安全性需求，因此在 Active Directory 中應該加以分隔。
2. PAW 電腦 GPO 應連結至實體和虛擬 PAW OU。
3. 為實體 PAW 建立新的 GPO，以將 PAW 使用者新增至 Hyper-V 系統管理員群組。 這是允許系統管理員連線到系統管理 VM，並視需要開啟/關閉它們的必要操作。 重要的是，讓登入實體 PAW 的使用者不具有系統管理員許可權、網際網路存取權、或是將惡意虛擬機器資料從網路共用或外部存放裝置複製到實體 PAW 的能力。
4. 為系統管理 VM 建立新的 GPO，以將 PAW 使用者新增至遠端桌面使用者群組。 這可讓使用者使用 Hyper-V 增強的主控台工作階段，以提供更佳的使用者體驗，並讓智慧卡通過 VM。

### <a name="set-up-the-host-guardian-service"></a>設定主機守護者服務

主機守護者服務 (HGS) 負責證明實體 PAW 裝置的身分識別與健康情況。
只有 HGS 已知的電腦和執行受信任[程式碼完整性原則](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)的電腦，才可以啟動受防護的 VM。
這有助於保護受防護的 VM 對抗使用者桌面環境的威脅，讓 VM 執行受信任的工作負載來管理您的階層式資源。

由於 HGS 負責判斷哪些裝置可以執行 PAW VM，因此會被視為第 0 層資源。
它應該與其他第 0 層資源一起部署，並受到保護，避免未經授權的實體和邏輯存取。
HGS 是叢集化的角色，可讓您輕鬆地相應放大任何大小的部署。
一般規則是為您擁有的每個 1,000 個裝置規劃 1 個 HGS 伺服器，且至少 3 個節點。

1. 若要安裝您的第一部 HGS 伺服器，請從 [安裝 HGS - 防禦樹系](../../security/guarded-fabric-shielded-vm/guarded-fabric-install-hgs-in-a-bastion-forest.md) 一文開始，並將 HGS 加入您的第 0 層網域。

2. 然後，使用您的企業憑證授權單位來[建立 HGS 的憑證](../../security/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs.md)。
擁有 HGS 加密並簽署憑證的任何人，都可以解密受防護的 VM，因此如果您可以存取硬體安全性模組來保護私密金鑰，建議您使用 HSM 來產生這些憑證。
為了更強的安全性，請選取大於或等於 4096 位元的金鑰大小。

3. 最後，遵循在 **TPM 模式**中[初始化 HGS 伺服器](../../security/guarded-fabric-shielded-vm/guarded-fabric-initialize-hgs-tpm-mode-bastion.md)的步驟。
初始化會設定您的 PAW 所使用的證明和金鑰保護 Web 服務。
HGS 應[設定 TLS 憑證](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-https.md)以保護這些通訊，而且從不受信任的網路到 HGS 只應開啟連接埠 443。

4. 遵循步驟為第二、第三和其他 HGS 節點[新增額外的節點](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-additional-hgs-nodes.md)。

5. 如果您的 HGS 伺服器是執行 Windows Server 2019 或更新版本，您可以啟用選擇性功能來快取 PAW 上受防護 VM 的金鑰，讓它們可以離線使用。 金鑰會被密封到系統目前的安全性設定，以防止有人在另一部電腦上使用快取的金鑰，或在不安全的狀態中使用同一部電腦。 如果您的 PAW 使用者在沒有網際網路存取的情況下移動，但仍然需要能夠登入其 PAW VM，這可能是很有用的解決方案。 若要使用這項功能，請在任何 HGS 伺服器上執行下列命令：

      ```powershell
      Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
      ```

### <a name="set-up-the-physical-paw-device"></a>設定實體 PAW 裝置

在受防護網狀架構解決方案中，預設將實體 PAW 裝置視為不受信任。
它可以在證明程式中證明它是值得信任的，然後就可以取得啟動受防護系統管理 VM 所需的金鑰。
裝置必須能夠執行 Hyper-V，並啟用安全開機和 TPM 2.0，以符合[受防護主機的必要條件](../../security/guarded-fabric-shielded-vm/guarded-fabric-guarded-host-prerequisites.md)。
支援所有 PAW 功能的最低作業系統版本是 **Windows 10 版本 1803**。

實體 PAW 的設定應該與任何其他虛擬機器一樣，唯一的例外是任何 PAW 使用者都必須是 Hyper-V 系統管理員，才能開啟系統管理 VM 並與其連線。
在您的無塵室環境中，必須替部署為系統管理 VM 受防護主機的每個唯一硬體/軟體組合，建立黃金設定。
在每個黃金設定中，完成下列工作：

1. 在電腦上安裝 Windows、驅動程式、韌體以及任何協力廠商管理或監視代理程式的最新更新。
2. [擷取必要的基準資訊](../../security/guarded-fabric-shielded-vm/guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)，包括電腦的唯一 TPM 識別碼 (簽署金鑰)、開機測量 (TCG 記錄)、程式碼完整性原則。
3. 將這些構件複製到 HGS 伺服器，並執行前一篇文章中的 HGS 證明命令以註冊主機。 如果您的所有主機都使用相同的程式碼完整性原則，以及/或使用相同的硬體設定，您只需要註冊程式碼完整性原則/TCG 記錄一次。

### <a name="create-the-signed-template-disk"></a>建立已簽署的範本磁碟

受防護的 VM 是使用已簽署的範本磁碟來建立。
簽章會在部署階段進行驗證，以便在將秘密 (例如系統管理員密碼) 釋放至 VM 之前，先確認磁碟完整性和真確性。

若要建立已簽署的範本磁碟，請在一般第 2 代虛擬機器上依照階段 1 部署步驟操作。
這部電腦將成為系統管理 VM 的黃金映像。
您可以建立不止一個範本磁碟，讓不同的情境能使用特製化的工具。

依需要設定 VM 後，執行 `C:\Windows\System32\sysprep\sysprep.exe` 並選擇將磁碟 [一般化]  。 當一般化完成時，[關閉]  作業系統。

最後，從 VM 的 VHDX 檔案執行 [範本磁碟精靈](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-template.md) 以安裝 BitLocker 元件並產生磁碟簽章。

#### <a name="create-the-shielding-data-file"></a>建立防護資料檔案

一般化的範本磁碟會與防護資料檔案配對，此檔案中包含佈建受防護 VM 所需的密碼。
防護資料檔案包含：
   - 守護者清單，定義允許 VM 執行的網狀架構。 每個 HGS 叢集都是它所保護之 PAW 裝置的守護者。
   - 部署信任的磁碟簽章清單。 防護資料檔案只會將其密碼釋放給使用授權來源媒體建立的 VM。
   - 安全性原則，規定是否應放入額外的保護來保護 VM 遠離主機，以及是否允許 VM 移至另一部電腦。 PAW 系統管理 VM 應一律受到全面防護。
   - unattend.xml 特製檔案，可讓 Windows 自動完成安裝，並包含像是本機系統管理員密碼之類的秘密。
   - 其他檔案，例如 RDP 或 VPN 憑證。

如需有關如何建立防護資料檔案的步驟，請參閱[防護資料檔案](../../security/guarded-fabric-shielded-vm/guarded-fabric-tenant-creates-shielding-data.md)一文。

受防護 VM 的擁有者金鑰非常敏感，應保留在 HSM 中，或離線儲存在安全的位置。
它們可以用在緊急中斷的脆弱案例中，不需要 HGS 就能啟動受防護的 VM。

強烈建議 PAW 系統管理 VM 的防護資料應包含將 VM 鎖定到其第一個開機所在實體主機的設定。
這可防止他人將系統管理 VM 從一個 PAW 移至相同環境中的另一個 PAW。
若要使用這項功能，使用 PowerShell 建立防護資料檔案，並包含 **BindToHostTpm** 參數：

```powershell
New-ShieldingDataFile -Policy Shielded -BindToHostTpm [...]
```

#### <a name="deploy-an-admin-vm"></a>部署系統管理 VM

準備好範本磁碟和防護資料檔案之後，您就可以在任何已向 HGS 註冊的 PAW 上部署系統管理 VM。

1. 將範本磁碟 (.vhdx) 和防護資料檔案 (.pdk) 複製到信任的 PAW 裝置。
2. 遵循指示，[使用 PowerShell 部署新的受防護 VM](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-using-powershell.md)。
3. 完成部署程序階段 1 的其餘步驟，以保護 VM 作業系統，並適當地為其預定角色進行設定。


## <a name="related-topics"></a>相關主題

[吸引人的 Microsoft 網路安全性服務](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx)

[頂級體驗︰如何減輕傳遞雜湊和其他形式認證竊取的風險](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[Microsoft 進階威脅分析](https://aka.ms/ata)

[使用 Credential Guard 保護衍生的網域認證](/windows/security/identity-protection/credential-guard/credential-guard)

[Device Guard 概觀](/windows/security/threat-protection/device-guard/introduction-to-device-guard-virtualization-based-security-and-windows-defender-application-control)

[使用安全的系統管理工作站保護高價值的資產](/previous-versions/mt186538(v=technet.10))

[Dave Probert 主講：Windows 10 中隔離的使用者模式 (第 9 頻道)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Logan Gabriel 主講：Windows 10 中隔離的使用者模式程序和功能 (第 9 頻道)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[Dave Probert 主講：深入探討 Windows 10 隔離的使用者模式中的程序和功能 (第 9 頻道)](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[使用 Windows 10 隔離的使用者模式來減少認證竊取 (第 9 頻道)](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[在 Windows Kerberos 中啟用嚴格 KDC 驗證](https://www.microsoft.com/download/details.aspx?id=6382)

[Windows Server 2012 Kerberos 驗證的新功能](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11))

[Windows Server 2008 R2 中 AD DS 的驗證機制保證逐步指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378897(v=ws.10))

[信賴平台模組技術概觀](/windows/device-security/tpm/trusted-platform-module-overview)
