---
title: 使用快速入門精靈部署單一 DirectAccess 伺服器
description: 瞭解使用單一 DirectAccess 伺服器的 DirectAccess 案例，並可讓您以幾個簡單的步驟部署 DirectAccess。
manager: brianlic
ms.topic: article
ms.assetid: eb0cf464-0668-40f8-8222-feb6bae6d3d5
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: e947f0a4a6c738937a315face5be97f0c509029a
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97944654"
---
# <a name="deploy-a-single-directaccess-server-using-the-getting-started-wizard"></a>使用快速入門精靈部署單一 DirectAccess 伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題提供使用單一 DirectAccess 伺服器並可讓您使用一些簡單的步驟部署 DirectAccess 的 DirectAccess 案例簡介。

## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>在開始部署之前，請參閱不支援的設定清單、已知問題和先決條件
部署 DirectAccess 之前，您可以使用下列主題來檢查必要條件和其他資訊。

-   [DirectAccess 不支援的設定](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)

-   [部署 DirectAccess 的必要條件](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)

## <a name="scenario-description"></a><a name="BKMK_OVER"></a>案例描述
在此案例中，執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的單一電腦，會在幾個簡單的 wizard 步驟中設定為具有預設設定的 DirectAccess 伺服器，而不需要設定基礎結構設定，例如 (CA) 或 Active Directory 安全性群組的憑證授權單位單位。

> [!NOTE]
> 如果您想要利用自訂設定來設定進階部署，請參閱 [Deploy a Single DirectAccess Server with Advanced Settings](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)

## <a name="in-this-scenario"></a>在這個案例中
若要設定基本 DirectAccess 伺服器，需要進行數個規劃和部署步驟。

### <a name="prerequisites"></a>先決條件
開始部署這個案例之前，請先檢閱這份重要需求清單：

-   必須在所有設定檔啟用 Windows 防火牆

-   只有當您的用戶端電腦正在執行 Windows 10、Windows 8.1 或 Windows 8 時，才支援此案例。

-   不支援公司網路中的 ISATAP。 如果您使用 ISATAP，則應該將它移除，然後使用原生的 IPv6。

-   公開金鑰基礎結構不是必要項目。

-   不支援部署雙因素驗證。 需要網域認證以進行驗證。

-   自動將 DirectAccess 部署到目前網域中的所有攜帶型電腦。

-   網際網路的流量不會通過 DirectAccess 通道。 不支援強制通道組態。

-   DirectAccess 伺服器是網路位置伺服器。

-   不支援「網路存取保護」(NAP)。

-   不支援在 DirectAccess 管理主控台以外或使用 PowerShell Cmdlet 來變更原則。

-   若要部署多網站，現在或未來部署，請先 [使用 Advanced Settings 部署單一 DirectAccess 伺服器](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。

### <a name="planning-steps"></a>規劃步驟
規劃可以分成兩個階段：

1.  為 DirectAccess 基礎結構做規劃。 這個階段描述在開始 DirectAccess 部署之前設定網路基礎結構所需的規劃。 其中包含規劃網路和伺服器拓撲，以及 DirectAccess 網路位置伺服器。

2.  為 DirectAccess 部署做規劃。 這個階段描述為 DirectAccess 部署做準備所需的規劃步驟。 它包含為 DirectAccess 用戶端電腦、伺服器和用戶端驗證需求、VPN 設定、基礎結構伺服器，以及管理和應用程式伺服器做規劃。

如需詳細的規劃步驟，請參閱 [規劃 Advanced DirectAccess 部署](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md)。

### <a name="deployment-steps"></a>部署步驟
部署可以分成三個階段：

1.  設定 DirectAccess 基礎結構-這個階段包括設定網路和路由、設定防火牆設定（如有必要）、設定憑證、DNS 伺服器、Active Directory 和 GPO 設定，以及 DirectAccess 網路位置伺服器。

2.  設定 DirectAccess 伺服器設定。 這個階段包含設定 DirectAccess 用戶端電腦、DirectAccess 伺服器、基礎結構伺服器、管理和應用程式伺服器的步驟。

3.  正在驗證部署。 這個階段包含驗證部署是否可依需求運作的步驟。

如需詳細的部署步驟，請參閱 [Install and Configure Basic DirectAccess](../../../remote-access/directaccess/single-server-wizard/Install-and-Configure-Basic-DirectAccess.md)。

## <a name="practical-applications"></a><a name="BKMK_APP"></a>實際應用
部署單一的遠端存取伺服器可以提供以下功能：

-   方便存取。 您可以將執行 Windows 10、Windows 8.1、Windows 8 或 Windows 7 的受管理用戶端電腦設定為 DirectAccess 用戶端。 這些用戶端可以隨時透過 DirectAccess 連接位於網際網路上的內部網路資源，無須登入 VPN 網路。 未執行上述作業系統的用戶端電腦可以使用傳統 VPN 連線，連線到內部網路。

-   易於管理。 「遠端存取」系統管理員可以透過 DirectAccess，從遠端管理網際網路上的 DirectAccess 用戶端電腦，即使用戶端電腦不位於公司內部網路上也可以。 管理伺服器可以自動修復不符合公司規定的用戶端電腦。 DirectAccess 和 VPN 都是在同一個主控台以及使用相同的一組精靈進行管理。 此外，只要一部遠端存取管理主控台就可以管理一或多部遠端存取伺服器。

## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>這個案例包含的角色與功能
下表列出本案例必備的角色和功能：

|角色/功能|如何支援本案例|
|---------|-----------------|
|遠端存取角色|這個角色是利用伺服器管理員主控台或 Windows PowerShell 安裝和解除安裝。 這個角色包含 DirectAccess (以前是 Windows Server 2008 R2 的功能)、 路由及遠端存取服務 (以前是網路原則與存取服務 (NPAS) 伺服器角色底下的角色服務)。 遠端存取角色包含兩個元件：<p>1. DirectAccess 與路由及遠端存取服務 (RRAS) VPN。 DirectAccess 和 VPN 會在遠端存取管理主控台中一起管理。<br />2. RRAS 路由。 RRAS 路由功能是在舊版路由和遠端存取主控台中管理。<p>遠端存取伺服器角色需要以下伺服器角色/功能：<p>-Internet Information Services (IIS) Web 服務器-在遠端存取服務器上設定網路位置伺服器和預設 Web 探查時，需要這項功能。<br />-Windows 內部資料庫。 用於遠端存取伺服器上的本機帳戶處理。|
|遠端存取管理工具功能|這個功能的安裝方式如下：<p>-安裝遠端存取角色時，預設會將它安裝在遠端存取服務器上，並支援遠端管理主控台使用者介面和 Windows PowerShell Cmdlet。<br />-您可以選擇性地將它安裝在未執行遠端存取服務器角色的伺服器上。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<p>遠端存取管理工具功能包含以下各項：<p>-遠端存取 GUI<br />-Windows PowerShell 的遠端存取模組<p>依存項目包括：<p>-群組原則管理主控台<br />-RAS 連線管理員系統管理組件 (CMAK) <br />-Windows PowerShell 3。0<br />-圖形化管理工具和基礎結構|

## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>硬體需求
本案例需要的硬體如下所示：

-   伺服器需求：

    -   符合 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 硬體需求的電腦。

    -   伺服器必須至少安裝並啟用一張網路介面卡，並連接到內部網路。 使用兩張介面卡時，應該有一張介面卡連接到內部公司網路，另一張連接到外部網路 (網際網路或私人網路)。

    -   至少一個網域控制站。 遠端存取伺服器和 DirectAccess 用戶端必須是網域成員。

-   用戶端需求：

    -   用戶端電腦必須執行 Windows 10、Windows 8.1 或 Windows 8。

        > [!IMPORTANT]
        > 如果您的部分或所有用戶端電腦執行的是 Windows 7，您必須使用 Advanced Setup Wizard。 本檔中所述的消費者入門安裝精靈不支援執行 Windows 7 的用戶端電腦。 如需有關如何搭配使用 Windows 7 用戶端與 DirectAccess 的指示，請參閱使用 [Advanced Settings 部署單一 DirectAccess 伺服器](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) 。

        > [!NOTE]
        > 只有下列作業系統可以用來做為 DirectAccess 用戶端： Windows 10 企業版、Windows 8.1 企業版、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 8 企業版、Windows Server 2008 R2、Windows 7 企業版和 Windows 7 旗艦版。

-   基礎結構和管理伺服器需求：

    -   如果啟用 VPN，而且未設定靜態 IP 位址集區，您必須部署 DHCP 伺服器以自動配置 IP 位址給 VPN 用戶端。

-   需要執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 SP2 或 Windows Server 2008 R2 的 DNS 伺服器。

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>軟體需求
本案例的一些需求：

-   伺服器需求：

    -   遠端存取伺服器必須是網域成員。 伺服器可以部署到內部網路的邊緣，或者在邊緣防火牆或其他裝置的後面。

    -   如果遠端存取伺服器是位在邊緣防火牆或 NAT 裝置後面，則裝置必須設定成允許遠端存取伺服器的連入和連出通訊。

    -   負責在伺服器部署「遠端存取」的人員，須具備伺服器的本機系統管理員身分以及擁有網域使用者權限。 此外，系統管理員需要具備部署 DirectAccess 所使用之 GPO 的權限。 如果想利用這些功能，將 DirectAccess 只部署到行動電腦上，必須具備在網域控制站建立 WMI 篩選器的權限。

-   遠端存取用戶端需求：

    -   DirectAccess 用戶端必須是網域成員。 包含用戶端的網域可以和遠端存取伺服器屬於相同的樹系，或者與遠端存取伺服器樹系存在雙向信任的關係。

    -   準備一個 Active Directory 安全性群組，只要會設定成 DirectAccess 用戶端的電腦，全部放到這個群組中。 如果在設定 DirectAccess 用戶端設定時未指定安全性群組，預設會在網域電腦安全性群組的所有膝上型電腦套用用戶端 GPO。 只有下列作業系統可以用來做為 DirectAccess 用戶端： Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows 8 企業版、Windows 7 企業版和 Windows 7 旗艦版。

## <a name="see-also"></a><a name="BKMK_LINKS"></a>請參閱
下表提供其他資源的連結。

|內容類型|參考|
|--------|-------|
|**TechNet 的遠端存取**|[遠端存取 TechCenter](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn756544(v=ws.11))|
|**工具及設定**|[遠端存取 PowerShell Cmdlet](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn756544(v=ws.11))|
|**社群資源**|[DirectAccess Wiki 內容](https://go.microsoft.com/fwlink/?LinkId=236871)|
|**相關技術**|[IPv6 的運作方式](/previous-versions/windows/it-pro/windows-server-2003/cc781672(v=ws.10))|

