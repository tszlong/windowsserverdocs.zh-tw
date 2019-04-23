---
title: 使用快速入門精靈部署單一 DirectAccess 伺服器
description: 本主題是指南部署單一 DirectAccess 伺服器使用取得啟動精靈的 Windows Server 2016 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eb0cf464-0668-40f8-8222-feb6bae6d3d5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d2bab7abd8acd958ccb0b0c1d64bc4245b61be36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854069"
---
# <a name="deploy-a-single-directaccess-server-using-the-getting-started-wizard"></a>使用快速入門精靈部署單一 DirectAccess 伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

本主題提供使用單一 DirectAccess 伺服器並可讓您使用一些簡單的步驟部署 DirectAccess 的 DirectAccess 案例簡介。  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>在開始部署之前，請參閱不支援的設定清單、已知問題和先決條件  
您可以使用下列主題來檢閱必要條件和其他資訊，然後再部署 DirectAccess。  
  
-   [DirectAccess 不支援的設定](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [部署 DirectAccess 的必要條件](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="BKMK_OVER"></a>案例描述  
在此案例中，執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，在單一電腦設定為 DirectAccess 伺服器中幾個簡單精靈步驟，而不需要設定基礎結構設定這類的預設設定為憑證授權單位 (CA) 或 Active Directory 安全性群組。  
  
> [!NOTE]  
> 如果您想要利用自訂設定來設定進階部署，請參閱 [Deploy a Single DirectAccess Server with Advanced Settings](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
  
## <a name="in-this-scenario"></a>在這個案例中  
若要設定基本 DirectAccess 伺服器，數個規劃和部署步驟則是必要項目。  
  
### <a name="prerequisites"></a>先決條件  
開始部署這個案例之前，請先檢閱這份重要需求清單：  
  
-   必須在所有設定檔啟用 Windows 防火牆  
  
-   當用戶端電腦正在執行 Windows 10，Windows 8.1 或 Windows 8 時，才支援此案例。  
  
-   不支援公司網路中的 ISATAP。 如果您使用 ISATAP，則應該將它移除，然後使用原生的 IPv6。  
  
-   公開金鑰基礎結構不是必要項目。  
  
-   不支援部署雙因素驗證。 需要網域認證以進行驗證。  
  
-   自動將 DirectAccess 部署到目前網域中的所有攜帶型電腦。  
  
-   網際網路的流量不會通過 DirectAccess 通道。 不支援強制通道組態。  
  
-   DirectAccess 伺服器是網路位置伺服器。  
  
-   不支援「網路存取保護」(NAP)。  
  
-   不支援在 DirectAccess 管理主控台以外或使用 PowerShell Cmdlet 來變更原則。  
  
-   若要部署多站台，現在或未來，第一次[部署單一 DirectAccess 伺服器使用進階設定](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
### <a name="planning-steps"></a>規劃步驟  
規劃可以分成兩個階段：  
  
1.  規劃 DirectAccess 基礎結構。 這個階段描述在開始 DirectAccess 部署之前設定網路基礎結構所需的規劃。 其中包含規劃網路和伺服器拓撲，以及 DirectAccess 網路位置伺服器。  
  
2.  為 DirectAccess 部署做規劃。 這個階段描述為 DirectAccess 部署做準備所需的規劃步驟。 它包含為 DirectAccess 用戶端電腦、伺服器和用戶端驗證需求、VPN 設定、基礎結構伺服器，以及管理和應用程式伺服器做規劃。  
  
如需詳細的規劃步驟，請參閱 <<c0> [ 規劃進階 DirectAccess 部署](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md)。  
  
### <a name="deployment-steps"></a>部署步驟  
部署可以分成三個階段：  
  
1.  設定 DirectAccess 基礎結構-這個階段包含設定網路和路由、 視需要設定憑證、 DNS 伺服器、 Active Directory 和 GPO 設定，以及 DirectAccess 網路位置設定防火牆設定伺服器。  
  
2.  設定 DirectAccess 伺服器設定。 這個階段包含設定 DirectAccess 用戶端電腦、DirectAccess 伺服器、基礎結構伺服器、管理和應用程式伺服器的步驟。  
  
3.  驗證部署。 這個階段包括確認部署正常運作所需的步驟。  
  
如需詳細的部署步驟，請參閱 [Install and Configure Basic DirectAccess](../../../remote-access/directaccess/single-server-wizard/Install-and-Configure-Basic-DirectAccess.md)。  
  
## <a name="BKMK_APP"></a>實際的應用程式  
部署單一的遠端存取伺服器可以提供以下功能：  
  
-   存取方便。 您可以設定為 DirectAccess 用戶端執行 Windows 10，Windows 8.1、 8 或 Windows 7 的受管理的用戶端電腦。 這些用戶端可以隨時透過 DirectAccess 連接位於網際網路上的內部網路資源，無須登入 VPN 網路。 未執行上述作業系統的用戶端電腦可以使用傳統 VPN 連線，連線到內部網路。  
  
-   管理容易。 「遠端存取」系統管理員可以透過 DirectAccess，從遠端管理網際網路上的 DirectAccess 用戶端電腦，即使用戶端電腦不位於公司內部網路上也可以。 管理伺服器可以自動修復不符合公司規定的用戶端電腦。 DirectAccess 和 VPN 都是在同一個主控台以及使用相同的一組精靈進行管理。 此外，只要一部遠端存取管理主控台就可以管理一或多部遠端存取伺服器。  
  
## <a name="BKMK_NEW"></a>在此案例包含角色與功能  
下表列出本案例必備的角色和功能：  
  
|角色/功能|如何支援本案例|  
|---------|-----------------|  
|遠端存取角色|這個角色是利用伺服器管理員主控台或 Windows PowerShell 安裝和解除安裝。 這個角色包含 DirectAccess (以前是 Windows Server 2008 R2 的功能)、 路由及遠端存取服務 (以前是網路原則與存取服務 (NPAS) 伺服器角色底下的角色服務)。 遠端存取角色包含兩個元件：<br /><br />1.DirectAccess 與路由及遠端存取服務 (RRAS) VPN。 DirectAccess 和 VPN，則會在遠端存取管理主控台中一起管理。<br />2.RRAS 路由。 在舊版的路由及遠端存取主控台中管理 RRAS 路由功能。<br /><br />遠端存取伺服器角色需要以下伺服器角色/功能：<br /><br />網際網路資訊服務 (IIS) Web Serve-這項功能，才能進行遠端存取伺服器和預設 web 探查的網路位置伺服器。<br />-Windows 內部資料庫。 用於遠端存取伺服器上的本機帳戶處理。|  
|遠端存取管理工具功能|這個功能的安裝方式如下：<br /><br />-它預設會安裝在遠端存取伺服器時的遠端存取角色安裝，並且可支援遠端管理主控台使用者介面和 Windows PowerShell cmdlet。<br />-它可以選擇性地安裝在不執行 遠端存取伺服器角色的伺服器。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<br /><br />遠端存取管理工具功能包含以下各項：<br /><br />-遠端存取 GUI<br />-適用於 Windows PowerShell 遠端存取模組<br /><br />依存項目包括：<br /><br />群組原則管理主控台<br />-RAS Connection Manager Administration Kit (CMAK)<br />-   Windows PowerShell 3.0<br />圖形化管理工具和基礎結構|  
  
## <a name="BKMK_HARD"></a>硬體需求  
本案例需要的硬體如下所示：  
  
-   伺服器需求：  
  
    -   符合 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的硬體需求的電腦。  
  
    -   伺服器必須至少安裝並啟用一張網路介面卡，並連接到內部網路。 使用兩張介面卡時，應該有一張介面卡連接到內部公司網路，另一張連接到外部網路 (網際網路或私人網路)。  
  
    -   至少一個網域控制站。 遠端存取伺服器和 DirectAccess 用戶端必須是網域成員。  
  
-   用戶端需求：  
  
    -   用戶端電腦必須執行 Windows 10，Windows 8.1 或 Windows 8。  
  
        > [!IMPORTANT]  
        > 如果部分或所有的用戶端電腦正在執行 Windows 7，您必須使用進階的安裝精靈。 開始使用安裝精靈這份文件中所述不支援執行 Windows 7 的用戶端電腦。 請參閱[部署單一 DirectAccess 伺服器使用進階設定](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)如需如何使用 Windows 7 用戶端與 DirectAccess 的指示。  
  
        > [!NOTE]  
        > 只有下列作業系統可以用來做為 DirectAccess 用戶端：Windows 10 企業版、 Windows 8.1 Enterprise、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows 8 企業版、 Windows Server 2008 R2、 Windows 7 Enterprise 和 Windows 7 Ultimate。  
  
-   基礎結構和管理伺服器需求：  
  
    -   如果啟用 VPN，而且未設定靜態 IP 位址集區，您必須部署 DHCP 伺服器以自動配置 IP 位址給 VPN 用戶端。  
  
-   執行 Windows Server 2016 的 DNS 伺服器，Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 SP2 或 Windows Server 2008 R2 需要。  
  
## <a name="BKMK_SOFT"></a>軟體需求  
本案例的一些需求：  
  
-   伺服器需求：  
  
    -   遠端存取伺服器必須是網域成員。 伺服器可以部署到內部網路的邊緣，或者在邊緣防火牆或其他裝置的後面。  
  
    -   如果遠端存取伺服器是位在邊緣防火牆或 NAT 裝置後面，則裝置必須設定成允許遠端存取伺服器的連入和連出通訊。  
  
    -   負責在伺服器部署「遠端存取」的人員，須具備伺服器的本機系統管理員身分以及擁有網域使用者權限。 此外，系統管理員需要具備部署 DirectAccess 所使用之 GPO 的權限。 如果想利用這些功能，將 DirectAccess 只部署到行動電腦上，必須具備在網域控制站建立 WMI 篩選器的權限。  
  
-   遠端存取用戶端需求：  
  
    -   DirectAccess 用戶端必須是網域成員。 包含用戶端的網域可以和遠端存取伺服器屬於相同的樹系，或者與遠端存取伺服器樹系存在雙向信任的關係。  
  
    -   準備一個 Active Directory 安全性群組，只要會設定成 DirectAccess 用戶端的電腦，全部放到這個群組中。 如果在設定 DirectAccess 用戶端設定時未指定安全性群組，預設會在網域電腦安全性群組的所有膝上型電腦套用用戶端 GPO。 只有下列作業系統可以用來做為 DirectAccess 用戶端：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012，Windows Server 2008 R2，Windows 8 Enterprise、 Windows 7 企業版和 Windows 7 Ultimate。  
  
## <a name="BKMK_LINKS"></a>另請參閱  
下表提供其他資源的連結。  
  
|內容類型|參考|  
|--------|-------|  
|**TechNet 上的遠端存取**|[遠端存取 TechCenter](https://technet.microsoft.com/network/bb545655.aspx)|  
|**工具及設定**|[遠端存取 PowerShell Cmdlet](https://technet.microsoft.com/library/hh918399.aspx)|  
|**社群資源**|[DirectAccess Wiki 項目](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**相關技術**|[IPv6 的運作方式](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  


