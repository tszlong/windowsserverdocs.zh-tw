---
title: 使用進階設定部署單一 DirectAccess 伺服器
description: 本主題是指南部署單一 DirectAccess 伺服器使用進階設定的 Windows Server 2016 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b211a9ca-1208-4e1f-a0fe-26a610936c30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f17e1c9dd1a4e2d064a4e5980904c524dc62fb72
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283605"
---
# <a name="deploy-a-single-directaccess-server-with-advanced-settings"></a>使用進階設定部署單一 DirectAccess 伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

本主題提供使用單一 DirectAccess 伺服器，並可讓您使用進階設定部署 DirectAccess 的 DirectAccess 案例簡介。  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>在開始部署之前，請參閱不支援的設定清單、已知問題和先決條件  
您可以使用下列主題來檢閱必要條件和其他資訊，然後再部署 DirectAccess。  
  
-   [DirectAccess 不支援的設定](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [部署 DirectAccess 的必要條件](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="BKMK_OVER"></a>案例描述  
在此案例中，執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，在單一電腦被設定為使用進階設定 DirectAccess 伺服器。  
  
> [!NOTE]  
> 如果您要設定只具有簡單設定的基本部署，請參閱＜ [Deploy a Single DirectAccess Server Using the Getting Started Wizard](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)＞。 在簡單案例中，會使用精靈以預設設定來設定 DirectAccess，而不需要設定基礎結構設定，例如憑證授權單位 (CA) 或 Active Directory 安全性群組。  
  
## <a name="in-this-scenario"></a>在這個案例中  
若要使用進階設定來設定單一 DirectAccess 伺服器，您必須完成數個規劃和部署步驟。  
  
### <a name="prerequisites"></a>先決條件  
在開始之前，您可以先檢閱下列需求。  
  
-   必須在所有設定檔啟用 Windows 防火牆。  
  
-   DirectAccess 伺服器是網路位置伺服器。  
  
-   您想要讓安裝 DirectAccess 伺服器之網域中的所有無線電腦都支援 DirectAccess。 部署 DirectAccess 時，它會自動在目前網域中的所有攜帶型電腦上啟用。  
  
> [!IMPORTANT]  
> 部署 DirectAccess 時，不支援某些技術和設定。  
>   
> -   不支援公司網路中的「內部網站自動通道定址通訊協定」(ISATAP)。 如果您使用 ISATAP，則必須將它移除，然後使用原生的 IPv6。  
  
### <a name="planning-steps"></a>規劃步驟  
規劃可以分成兩個階段：  
  
1.  **為 DirectAccess 基礎結構做規劃**。 這個階段描述在開始 DirectAccess 部署之前設定網路基礎結構所需的規劃。 它包含規劃網路和伺服器拓撲、憑證規劃、DNS、Active Directory 及群組原則物件 (GPO) 設定，以及 DirectAccess 網路位置伺服器。  
  
2.  **為 DirectAccess 部署做規劃**。 這個階段描述為 DirectAccess 部署做準備所需的規劃步驟。 它包含為 DirectAccess 用戶端電腦、伺服器和用戶端驗證需求、VPN 設定、基礎結構伺服器，以及管理和應用程式伺服器做規劃。  
  
### <a name="deployment-steps"></a>部署步驟  
部署可以分成三個階段：  
  
1.  **設定 DirectAccess 基礎結構**。 這個階段包含設定網路和路由、視需要設定防火牆設定、設定憑證、DNS 伺服器、Active Directory 和 GPO 設定，以及 DirectAccess 網路位置伺服器。  
  
2.  **設定 DirectAccess 伺服器設定**。 這個階段包含設定 DirectAccess 用戶端電腦、DirectAccess 伺服器、基礎結構伺服器、管理和應用程式伺服器的步驟。  
  
3.  **確認部署**。 這個階段包括確認 DirectAccess 部署的步驟。  
  
如需詳細的部署步驟，請參閱＜ [Install and Configure Advanced DirectAccess](../../../remote-access/directaccess/single-server-advanced/Install-and-Configure-Advanced-DirectAccess.md)＞。  
  
## <a name="BKMK_APP"></a>實際的應用程式  
部署單一 DirectAccess 伺服器可提供下列好處：  
  
-   **容易存取**。 執行 Windows 10，Windows 8.1，Windows 8 和 Windows 7 的受管理的用戶端電腦可以設定成 DirectAccess 用戶端電腦。 這些用戶端可以隨時透過 DirectAccess 連接位於網際網路上的內部網路資源，無須登入 VPN 網路。 未執行上述作業系統的用戶端電腦可以透過 VPN 連接內部網路。  
  
-   **容易管理**。 「遠端存取」系統管理員可以透過 DirectAccess，從遠端管理網際網路上的 DirectAccess 用戶端電腦，即使用戶端電腦不位於公司內部網路上也可以。 管理伺服器可以自動修復不符合公司規定的用戶端電腦。 DirectAccess 和 VPN 都是在同一個主控台以及使用相同的一組精靈進行管理。 此外，只要單一的「遠端存取管理主控台」就可以管理一或多部 DirectAccess 伺服器。  
  
## <a name="BKMK_NEW"></a>角色與此案例所需的功能  
下表列出本案例所需的角色與功能：  
  
|角色/功能|如何支援本案例|  
|---------|-----------------|  
|遠端存取角色|這個角色是利用伺服器管理員主控台或 Windows PowerShell 安裝和解除安裝。 這個角色同時包含 DirectAccess 和「路由及遠端存取服務」(RRAS)。 遠端存取角色包含兩個元件：<br/><br/>1.DirectAccess 和 RRAS VPN。 DirectAccess 和 VPN，則會在遠端存取管理主控台中一起管理。<br/>2.RRAS 路由。 在舊版的路由及遠端存取主控台中管理 RRAS 路由功能。<br /><br />「遠端存取」伺服器角色取決於下列伺服器角色/功能：<br/><br/> 網際網路資訊服務 (IIS) Web Serve-這項功能，才能在 DirectAccess 伺服器和預設 web 探查設定網路位置伺服器。<br/> -Windows 內部資料庫。 用於 DirectAccess 伺服器上的本機計量。|  
|遠端存取管理工具功能|這個功能的安裝方式如下：<br /><br />-它會安裝在 DirectAccess 伺服器上的預設值時的遠端存取角色安裝，並且可支援遠端管理主控台使用者介面和 Windows PowerShell cmdlet。<br />-它可以選擇性地安裝在未執行 DirectAccess 伺服器角色的伺服器。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<br /><br />遠端存取管理工具功能包含以下各項：<br /><br />-遠端存取圖形化使用者介面 (GUI)<br />-適用於 Windows PowerShell 遠端存取模組<br /><br />依存項目包括：<br /><br />群組原則管理主控台<br />-RAS Connection Manager Administration Kit (CMAK)<br />-   Windows PowerShell 3.0<br />圖形化管理工具和基礎結構|  
  
## <a name="BKMK_HARD"></a>硬體需求  
本案例需要的硬體如下所示：  
  
-   伺服器需求：  
  
    -   符合 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的硬體需求的電腦。  
  
    -   伺服器必須至少安裝並啟用一張網路介面卡，並連接到內部網路。 使用兩張介面卡時，應該有一張介面卡連接到內部公司網路，另一張連接到外部網路 (網際網路或私人網路)。  
  
    -   如果 Teredo 必須當作 IPv4 到 IPv6 的轉換通訊協定，則伺服器的外部介面卡需要兩個連續的公用 IPv4 位址。 如果只有一個 IP 位址可用，則只能使用 IP-HTTPS 做為轉換通訊協定。  
  
    -   至少一個網域控制站。 DirectAccess 伺服器和 DirectAccess 用戶端必須是網域成員。  
  
    -   如果您不想要將自我簽署憑證用於 IP-HTTPS 或網路位置伺服器，或是想要使用用戶端憑證來進行用戶端 IPsec 驗證，則需要憑證授權單位 (CA)。 您也可以從公用 CA 要求憑證。  
  
    -   如果網路位置伺服器不是位於 DirectAccess 伺服器上，則需要一個個別的網頁伺服器來執行它。  
  
-   用戶端需求：  
  
    -   用戶端電腦必須執行 Windows 10、windows 8 或 Windows 7。  
  
        > [!NOTE]  
        > 為 DirectAccess 用戶端，可以使用下列作業系統：Windows 10、 Windows Server 2012 R2、 Windows Server 2012 中，Windows 8 企業版、 Windows 7 Enterprise 或 Windows 7 Ultimate。  
  
-   基礎結構和管理伺服器需求：  
  
    -   從遠端管理 DirectAccess 用戶端電腦時，用戶端會與管理伺服器進行通訊，例如網域控制站、系統中心設定伺服器以及健康情況登錄授權單位 (HRA) 伺服器，以便使用各種服務，其中包含 Windows 和防毒更新以及網路存取保護 (NAP) 用戶端規範。 開始部署「遠端存取」之前，應該先部署必要的伺服器。  
  
    -   如果「遠端存取」需要用戶端 NAP 規範，則開始部署「遠端存取」之前，應該先部署 NPS 和 HRS 伺服器。  
  
    -   如果啟用 VPN，而且在未使用靜態位址集區的情況下，需要 DHCP 伺服器自動分配 IP 給 VPN 用戶端。  
  
## <a name="BKMK_SOFT"></a>軟體需求  
本案例的一些需求：  
  
-   伺服器需求：  
  
    -   DirectAccess 伺服器必須是網域成員。 伺服器可以部署到內部網路的邊緣，或者在邊緣防火牆或其他裝置的後面。  
  
    -   如果 DirectAccess 伺服器位於邊緣防火牆或 NAT 裝置後面，則必須設定裝置來允許連到 DirectAccess 伺服器和從此伺服器連入的流量。  
  
    -   負責在伺服器部署「遠端存取」的人員，須具備伺服器的本機系統管理員身分以及擁有網域使用者權限。 此外，系統管理員需要具備部署 DirectAccess 所使用之 GPO 的權限。 如果想利用這些功能，將 DirectAccess 只部署到行動電腦上，必須具備在網域控制站建立 WMI 篩選器的權限。  
  
-   遠端存取用戶端需求：  
  
    -   DirectAccess 用戶端必須是網域成員。 包含用戶端的網域可以與 DirectAccess 伺服器屬於相同的樹系，或是與 DirectAccess 伺服器樹系或網域具有雙向信任關係。  
  
    -   準備一個 Active Directory 安全性群組，只要會設定成 DirectAccess 用戶端的電腦，全部放到這個群組中。 如果在設定 DirectAccess 用戶端設定時未指定安全性群組，預設會在網域電腦安全性群組的所有膝上型電腦套用用戶端 GPO。  
  
        > [!NOTE]  
        > 建議您為每個包含 DirectAccess 用戶端電腦的網域都建立一個安全性群組。  
  
        > [!IMPORTANT]  
        > 如果您已啟用 Teredo 在 DirectAccess 部署中，而且您想要提供存取至 Windows 7 用戶端，請確定用戶端會升級到 Windows 7 SP1。 使用 Windows 7 RTM 的用戶端無法透過 Teredo 進行連線。 不過，這些用戶端仍然可以透過 IP-HTTPS 連線到公司網路。  
  
## <a name="BKMK_LINKS"></a>另請參閱  
下表提供其他資源的連結。  
  
|內容類型|參考|  
|--------|-------|  
|**部署**|[Windows Server 中的 DirectAccess 部署路徑](../../../remote-access/directaccess/DirectAccess-Deployment-Paths-in-Windows-Server.md)<br /><br />[部署單一 DirectAccess 伺服器使用開始使用的精靈](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|  
|**工具及設定**|[遠端存取 PowerShell Cmdlet](https://technet.microsoft.com/library/hh918399.aspx)|  
|**社群資源**|[DirectAccess 存續指南](https://social.technet.microsoft.com/wiki/contents/articles/23210.directaccess-survival-guide.aspx)<br /><br />[DirectAccess Wiki 項目](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**相關技術**|[IPv6 的運作方式](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  


