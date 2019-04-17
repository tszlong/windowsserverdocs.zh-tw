---
title: 部署 Always On VPN
description: 本主題提供部署 Always On VPN Windows Server 2016 中的詳細的指示。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15c60f2986d3837c5c6e03f9e0a25c7e0a4e0cc0
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067133"
---
# 部署 Always On VPN

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #0171;[**前一個：** 了解有關 Always On VPN 進階功能](always-on-vpn-adv-options.md)<br>
& #0187;[**下一步：** 步驟 1。開始規劃 Always On VPN 部署](always-on-vpn-deploy-planning.md)

在本節中，您會了解部署 Always On VPN 連線的遠端加入網域的 Windows 10 用戶端電腦的工作流程。 如果您想要**設定的條件式存取**來微調 VPN 使用者如何存取您的資源時，請參閱[適用於 VPN 連線使用 Azure AD 的條件式存取](../../ad-ca-vpn-connectivity-windows10.md)。 若要深入了解使用 Azure AD 的 VPN 連線的條件式存取，請參閱[Azure Active Directory 中的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。 


下圖說明不同案例的工作流程程序部署 Always On VPN 時： 

_按一下以放大影像_。

<a href="../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow.png" alt="Full-sized view of the Always On VPN deployment workflow" target="_blank">![thumbnail](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)
</a> 

>[!IMPORTANT]
>如需此部署中，這不是假設您的基礎結構伺服器，例如電腦執行 Active Directory 網域服務、 Active Directory 憑證服務及網路原則伺服器，執行 Windows Server 2016 需求。 您可以使用較舊版本的 Windows Server，例如 Windows Server 2012 R2，伺服器所需的基礎結構及執行遠端存取伺服器。

## [步驟 1. 規劃 Always On VPN 部署](always-on-vpn-deploy-planning.md)

在此步驟中，您開始規劃和準備您的 Always On VPN 部署。 在您的電腦上安裝遠端存取伺服器角色之前您打算使用為 VPN 伺服器。 適當計畫之後，您可以部署 Always On VPN，並選擇性設定的 VPN 連線使用 Azure AD 的條件式存取。

## [步驟 2. 設定 Always On VPN 伺服器基礎結構](vpn-deploy-server-infrastructure.md)

在此步驟中，您可以安裝及設定支援 VPN 所需的伺服器端元件。 伺服器端元件，包括設定 PKI 散發的使用者、 VPN 伺服器和 NPS 伺服器所使用的憑證。  您也可以設定 RRAS 支援 IKEv2 連線和 NPS 伺服器執行適用於 VPN 連線的授權。

若要設定伺服器基礎結構，您必須執行下列工作：
- **使用 Active Directory 網域服務設定在伺服器上：** 啟用憑證自動註冊群組原則中的電腦與使用者、 建立 VPN 使用者群組，VPN 伺服器群組，以及 NPS 伺服器群組，以及新增至每個群組的成員。
- **上 Active Directory 憑證伺服器 CA:** 建立使用者驗證、 VPN 伺服器驗證，以及 NPS 伺服器驗證憑證範本。
- **加入網域的 Windows 10 用戶端上：** 註冊和驗證使用者的憑證。

## [步驟 3. 設定 Always On VPN 的遠端存取伺服器](vpn-deploy-ras.md)

在此步驟中，您可以設定遠端存取 VPN 允許 IKEv2 VPN 連線、 拒絕來自其他 VPN 通訊協定，連線，並指派授權的 VPN 用戶端連線的 IP 位址發行靜態 IP 位址集區。

若要設定 RAS，您必須執行下列工作：
- 註冊和驗證 VPN 伺服器憑證
- 安裝與設定遠端存取 VPN

## [步驟 4. 安裝並設定 NPS 伺服器](vpn-deploy-nps.md)

在此步驟中，您可以安裝網路原則伺服器 (NPS) 藉由使用 Windows PowerShell 或伺服器管理員新增角色與功能精靈。 您也可以設定 NPS 以處理所有驗證、 授權和計量執行任務，它會收到來自 VPN 伺服器的連線要求。

若要設定 NPS，您必須執行下列工作：
- 在 Active Directory 中登錄 NPS 伺服器
- 設定 RADIUS 計量 NPS 伺服器
- 新增為 RADIUS 用戶端在 NPS 中的 VPN 伺服器
- 在 NPS 中設定網路原則
- NPS 伺服器憑證自動註冊

## [步驟 5. 設定 DNS 及防火牆設定一律在 VPN](vpn-deploy-dns-firewall.md)

在此步驟中，您可以設定 DNS 和防火牆設定。 在遠端 VPN 用戶端連線時，它們會使用您的內部用戶端所使用的相同 DNS 伺服器，可讓他們能夠解析名稱做為您的內部工作站的其餘部分的相同方式集。 

## [步驟 6. 設定 Windows 10 用戶端 Always On VPN 連線](vpn-deploy-client-vpn-connections.md)

在此步驟中，您可以設定 VPN 連線使用該基礎結構與通訊的 windows 10 用戶端電腦。 您可以使用數種技術來設定 windows 10 VPN 用戶端，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune。 所有三個需要設定適當的 VPN 設定 XML VPN 設定檔。 

## [步驟 7. （選擇性）設定 VPN 連線的條件式的存取](../../ad-ca-vpn-connectivity-windows10.md) 
您可以在此選用步驟，來微調如何已獲授權的 VPN 使用者存取您的資源。 使用 Azure AD 的 VPN 連線的條件式存取，您可以協助保護 VPN 連線。 條件式存取 」 是原則型評估引擎，可讓您為任何 Azure AD 中建立存取規則已連線的應用程式。 如需詳細資訊，請參閱[Azure Active Directory (Azure AD) 的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。


## 後續步驟
[步驟 1。規劃 Always On VPN 部署](always-on-vpn-deploy-planning.md)︰ 您打算使用為 VPN 伺服器的電腦上安裝遠端存取伺服器角色之前。 適當計畫之後，您可以部署 Always On VPN，並選擇性設定的 VPN 連線使用 Azure AD 的條件式存取。  



---