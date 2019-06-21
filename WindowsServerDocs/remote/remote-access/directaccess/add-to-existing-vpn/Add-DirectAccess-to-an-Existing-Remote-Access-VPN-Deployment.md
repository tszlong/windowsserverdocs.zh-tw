---
title: 將 DirectAccess 新增至現有的遠端存取 (VPN) 部署
description: 本主題是本指南新增 DirectAccess 加入現有的遠端存取 (VPN) 部署適用於 Windows Server 2016 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5db01f7-1ae0-46f2-9be7-8d9e121446b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c6bd11855ec9c5387241ba42babf88c0461a9c4
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283578"
---
# <a name="add-directaccess-to-an-existing-remote-access-vpn-deployment"></a>將 DirectAccess 新增至現有的遠端存取 (VPN) 部署

>適用於：Windows Server （半年通道），Windows Server 2016
  
## <a name="BKMK_OVER"></a>案例描述  
在此案例中，單一電腦執行 Windows Server 2016 中，Windows Server 2012 R2 或 Windows Server 2012 被設定為 DirectAccess 伺服器使用建議的設定之後您已經安裝並設定 VPN。 如果您想要在 DirectAccess 設定企業功能，例如負載平衡叢集、 多站台的部署或雙因素用戶端驗證時，完成 設定單一伺服器，本主題中所述的案例，然後再將企業中所述的案例[在企業中部署遠端存取](../../ras/Deploy-Remote-Access-in-an-Enterprise.md)。  
  
## <a name="in-this-scenario"></a>在這個案例中  
若要設定單一遠端存取伺服器，需要執行許多規劃和部署步驟。  
  
### <a name="planning-steps"></a>規劃步驟  
規劃可以分成兩個階段：  
  
1.  **規劃遠端存取基礎結構**  
  
    在此階段中，開始遠端存取部署之前，您要先描述設定網路基礎結構所需的規劃。 它包含規劃網路和伺服器拓撲、憑證、網域名稱系統 (DNS)、Active Directory 和群組原則物件 (GPO) 設定，以及 DirectAccess 網路位置伺服器。  
  
2.  **規劃遠端存取部署**  
  
    在此階段中，您要描述準備遠端存取部署所需的步驟。 它包含規劃遠端存取用戶端電腦、伺服器及用戶端驗證需求，以及基礎結構伺服器。  
  
### <a name="deployment-steps"></a>部署步驟  
部署可以分成三個階段：  
  
1.  **設定遠端存取基礎結構**  
  
    在此階段中，您要設定網路及路由、防火牆設定 (如果需要)、憑證、DNS 伺服器、Active Directory 和 GPO 設定，以及 DirectAccess 網路位置伺服器。  
  
2.  **設定遠端存取伺服器設定**  
  
    在此階段中，您要設定遠端存取用戶端電腦、遠端存取伺服器以及基礎結構伺服器。  
  
3.  **確認部署**  
  
    在此階段中，您要確認部署如預期運作。  
  
## <a name="BKMK_APP"></a>實際的應用程式  
部署單一的遠端存取伺服器可以提供以下功能：  
  
-   **輕鬆存取**  
  
    管理執行 Windows 8 和 Windows 7 的電腦可以設定成 DirectAccess 用戶端電腦的用戶端。 這些用戶端在網際網路上可以隨時透過 DirectAccess 存取內部網路資源，無須登入 VPN 連線。 未執行上述作業系統的用戶端電腦可以透過 VPN 連線到內部網路。 DirectAccess 和 VPN 都在同一個主控台，以及使用相同的一組精靈進行管理。  
  
-   **容易管理**  
  
    遠端存取管理員可以使用 DirectAccess，遠端管理能存取網際網路的 DirectAccess 用戶端電腦，即使用戶端電腦並非位於公司內部網路亦同。 管理伺服器可以自動修復不符合公司規定的用戶端電腦。  
  
## <a name="BKMK_NEW"></a>角色與此案例所需的功能  
下表列出本案例所需的角色與功能：  
  
|角色/功能|如何支援本案例|  
|---------|-----------------|  
|遠端存取角色|使用伺服器管理員主控台或 Windows PowerShell 安裝和解除安裝角色。 這個角色包含 DirectAccess (以前是 Windows Server 2008 R2 的功能) 與路由及遠端存取服務 (以前是網路原則與存取服務 (NPAS) 伺服器角色底下的角色服務)。 遠端存取角色包含兩個元件：<br /><br />1.DirectAccess 與路由及遠端存取服務 (RRAS) VPN：在遠端存取管理主控台中管理。<br />2.RRAS 路由：在路由及遠端存取主控台中管理。<br /><br />遠端存取伺服器角色需要下列伺服器功能：<br /><br />網際網路資訊服務 (IIS) Web 伺服器： 必須設定網路位置伺服器上的遠端存取伺服器和預設 web 探查。<br />-Windows 內部資料庫：用於遠端存取伺服器上的本機帳戶處理。|  
|遠端存取管理工具功能|這個功能的安裝方式如下：<br /><br />-根據預設，安裝遠端存取角色時，遠端存取伺服器上。 支援遠端管理主控台使用者介面與 Windows PowerShell Cmdlet。<br />-選擇性地安裝在並未執行 遠端存取伺服器角色的伺服器上。 在此情況下，它是用於遠端管理執行 DirectAccess 和 VPN 的遠端存取電腦。<br /><br />遠端存取管理工具功能包含以下各項：<br /><br />-遠端存取 GUI<br />-適用於 Windows PowerShell 遠端存取模組<br /><br />依存項目包括：<br /><br />群組原則管理主控台<br />-RAS Connection Manager Administration Kit (CMAK)<br />-   Windows PowerShell 3.0<br />圖形化管理工具和基礎結構|  
  
## <a name="BKMK_HARD"></a>硬體需求  
本案例需要的硬體如下所示：  
  
**伺服器需求**  
  
-   符合 Windows Server 2012 的硬體需求的電腦。  
  
-   伺服器必須至少安裝並啟用一張網路介面卡，且加入內部網路。 使用兩張介面卡時，必須有一張介面卡連線到公司內部網路，另一張連線到外部網路 (網際網路)。  
  
-   如果 Teredo 必須當作 IPv4 到 IPv6 的轉換通訊協定，則伺服器的外部介面卡需要兩個連續的公用 IPv4 位址。 啟用 DirectAccess 精靈不會啟用 Teredo，即使有兩個連續 IP 位址亦同。 如果只有單一 IP 位址，則只能使用 IP-HTTPS 做為轉換通訊協定。  
  
-   至少一個網域控制站。 遠端存取伺服器和 DirectAccess 用戶端必須是網域成員。  
  
-   啟用 DirectAccess 精靈需要 IP-HTTPS 的憑證和網路位置伺服器。 如果 SSTP VPN 已經使用憑證，就會為 IP-HTTPS 重複使用。 如果未設定 SSTP VPN，您可以設定 IP-HTTPS 的憑證，或使用自動建立的自我簽署憑證。 對於網路位置伺服器，您可以設定憑證，或使用自動建立的自我簽署憑證。  
  
**用戶端需求**  
  
-   用戶端電腦必須執行 Windows 8 或 Windows 7。  
  
    > [!NOTE]  
    > 只有下列作業系統可以用來做為 DirectAccess 用戶端：Windows Server 2012、 Windows Server 2008 R2、 Windows 8 企業版、 Windows 7 Enterprise 和 Windows 7 Ultimate。  
  
**基礎結構和管理伺服器需求**  
  
-   遠端管理 DirectAccess 用戶端電腦時，用戶端會與管理伺服器進行通訊，例如網域控制站、系統中心設定伺服器以及健康情況登錄授權單位 (HRA) 伺服器，以便使用各種服務，其中包含 Windows 和防毒更新以及網路存取保護 (NAP) 用戶端規範。 開始部署遠端存取之前，必須先部署必要的伺服器。  
  
-   如果遠端存取需要用戶端 NAP 相容性，開始部署遠端存取之前，必須先部署網路原則伺服器 (NPS) 與 HRA。  
  
-   需要執行 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 SP2 的 DNS 伺服器。  
  
## <a name="BKMK_SOFT"></a>軟體需求  
本案例的軟體需求如下：  
  
**伺服器需求**  
  
-   遠端存取伺服器必須是網域成員。 伺服器可以部署到內部網路的邊緣，或者在邊緣防火牆或其他裝置的後面。  
  
-   如果遠端存取伺服器位在邊緣防火牆或網路位址轉譯 (NAT) 裝置後面，則裝置必須設定成允許流量進出遠端存取伺服器。  
  
-   負責在伺服器部署遠端存取的人員，必須具備伺服器的本機系統管理員身分，並且擁有網域使用者權限。 此外，系統管理員需要具備部署 DirectAccess 所使用之 GPO 的權限。 如果想利用這些功能，讓 DirectAccess 部署只限於攜帶型電腦，必須具備在網域控制站建立 WMI 篩選器的權限。  
  
**遠端存取用戶端需求**  
  
-   DirectAccess 用戶端必須是網域成員。 包含用戶端的網域可以和遠端存取伺服器屬於相同的樹系，或者與遠端存取伺服器樹系或網域存在雙向信任的關係。  
  
-   準備一個 Active Directory 安全性群組，只要會設定成 DirectAccess 用戶端的電腦，全部放到這個群組中。 如果在設定 DirectAccess 用戶端設定時未指定安全性群組，預設會在 Domain Computers 安全性群組的所有膝上型電腦 (具備 DirectAccess 功能) 套用用戶端 GPO。 只有下列作業系統可以用來做為 DirectAccess 用戶端：Windows Server 2012、 Windows Server 2008 R2、 Windows 8 企業版、 Windows 7 Enterprise 和 Windows 7 Ultimate。  
  
    > [!NOTE]  
    > 我們建議您為每個包含設定為 DirectAccess 用戶端電腦的網域建立安全性群組。  
  

  

