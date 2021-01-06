---
title: 部署 802.1 X 有線和無線部署的伺服器憑證
description: 本主題是適用于 802.1 X 有線和無線部署的指南部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 844a64558d4822c26effddc4b3772ad0a191718c
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947844"
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>部署 802.1 X 有線和無線部署的伺服器憑證

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本指南，將伺服器憑證部署到遠端存取和網路原則伺服器 (NPS) 基礎結構伺服器。

本指南涵蓋下列各節。

-   [使用本指南的必要條件](#bkmk_pre)

-   [本指南中未提供的內容](#bkmk_not)

-   [技術概覽](#bkmk_tech)

-   [伺服器憑證部署總覽](Server-Certificate-Deployment-Overview.md)

-   [伺服器憑證部署規劃](Server-Certificate-Deployment-Planning.md)

-   [伺服器憑證部署](Server-Certificate-Deployment.md)

### <a name="digital-server-certificates"></a>**數位伺服器憑證**
本指南提供使用 Active Directory 憑證服務 (AD CS) 將憑證自動註冊至遠端存取和 NPS 基礎結構伺服器的指示。 AD CS 可讓您建立 (PKI) 的公開金鑰基礎結構，並為您的組織提供公開金鑰密碼編譯、數位憑證和數位簽章功能。

當您在網路上的電腦之間使用數位伺服器憑證進行驗證時，憑證會提供：

1. 加密的機密性。
2. 透過數位簽章的完整性。
3. 將憑證金鑰與電腦網路上的電腦、使用者或裝置帳戶建立關聯，以進行驗證。

### <a name="server-types"></a>**伺服器類型**
您可以使用本指南，將伺服器憑證部署到下列類型的伺服器。
- 執行遠端存取服務的伺服器，也就是 DirectAccess 或標準虛擬私人網路 (VPN) 伺服器，而且是 [ **RAS 和 資訊存取伺服器** ] 群組的成員。
- 執行網路原則伺服器 (NPS) 服務的伺服器，其為 **RAS 和 資訊存取伺服器** 群組的成員。

### <a name="advantages-of-certificate-autoenrollment"></a>**憑證自動註冊的優點**
自動註冊伺服器憑證（也稱為「自動註冊」）提供下列優點。

- AD CS 憑證授權單位單位 (CA) 會自動向所有 NPS 和遠端存取服務器註冊伺服器憑證。
- 網域中的所有電腦都會自動接收您的 CA 憑證，此憑證會安裝在每個網域成員電腦上的 [信任的根憑證授權單位] 存放區中。 因此，網域中的所有電腦都信任您 CA 所發行的憑證。 這項信任可讓您的驗證服務器向彼此證明其身分識別，並參與安全通訊。
- 除了重新整理群組原則之外，並不需要手動重新設定每一部伺服器。
- 每個伺服器憑證都在增強金鑰使用方法中同時包含伺服器驗證目的和用戶端驗證用途 (EKU) 擴充功能。
- 延展性。 在部署您的企業根 CA 與本指南之後，您可以藉由新增企業次級 Ca， (PKI) 擴充您的公開金鑰基礎結構。
- 管理性。 您可以使用 AD CS 主控台或 Windows PowerShell 命令和腳本來管理 AD CS。
- 簡單。 您可以使用 Active Directory 群組帳戶和群組成員資格來指定註冊伺服器憑證的伺服器。
- 當您部署伺服器憑證時，憑證會以您使用本指南中的指示設定的範本為基礎。 這表示您可以針對特定的伺服器類型自訂不同的憑證範本，也可以針對您要發出的所有伺服器憑證使用相同的範本。

## <a name="prerequisites-for-using-this-guide"></a><a name="bkmk_pre"></a>使用本指南的必要條件

本指南提供如何在 Windows Server 2016 中使用 AD CS 和 Web 服務器 (IIS) server 角色來部署伺服器憑證的指示。 以下是執行本指南中程式的必要條件。

- 您必須使用 Windows Server 2016 核心網路指南部署核心網路，或者您必須已經在網路上安裝和正常運作的核心網路指南中所提供的技術。 這些技術包括 TCP/IP v4、DHCP、Active Directory Domain Services (AD DS) 、DNS 和 NPS。
  >[!NOTE]
  >Windows server 2016 核心網路指南適用于 Windows Server 2016 技術文件庫。 如需詳細資訊，請參閱 [核心網路指南](../../../core-network-guide/Core-Network-Guide.md)。

- 您必須閱讀本指南的規劃一節，以確定您已準備好進行此部署，再執行部署。
- 您必須依照顯示的循序執行本指南中的步驟。 請勿事先部署 CA，而不需要執行導致部署伺服器的步驟，否則您的部署將會失敗。
- 您必須準備好在您的網路上部署兩部新的伺服器：一部伺服器，您將在此伺服器上安裝 AD CS 作為企業根 CA，而一部伺服器將安裝 Web 服務器 (IIS) 讓您的 CA 可將憑證撤銷清單 (CRL) 發佈至 Web 服務器。

>[!NOTE]
>您已經準備好將靜態 IP 位址指派給 Web 和您使用本指南部署的 AD CS 伺服器，以及根據您組織的命名慣例來命名電腦。 此外，您必須將電腦加入網域。

## <a name="what-this-guide-does-not-provide"></a><a name="bkmk_not"></a>本指南中未提供的內容
本指南不提供完整的指示，說明如何使用 AD CS (PKI) 來設計和部署公開金鑰基礎結構。 建議您先複習 AD CS 檔和 PKI 設計檔，再部署本指南中的技術。

## <a name="technology-overviews"></a><a name="bkmk_tech"></a>技術概覽
以下是 AD CS 和 Web 服務器 (IIS) 的技術總覽。

### <a name="active-directory-certificate-services"></a>Active Directory 憑證服務
Windows Server 2016 中的 AD CS 提供可自訂的服務，可用於建立和管理在採用公開金鑰技術的軟體安全性系統中所使用的 x.509 憑證。 組織可以使用 AD CS 將人員、裝置或服務的身分識別系結至對應的公開金鑰，以增強安全性。 AD CS 也包含其他功能，讓您管理各種彈性環境中的憑證註冊與撤銷。

如需詳細資訊，請參閱 [Active Directory 憑證服務總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831740(v=ws.11)) 和 [公開金鑰基礎結構設計指引](https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/designing-and-implementing-a-pki-part-i-design-and-planning/ba-p/396953)。

### <a name="web-server-iis"></a>Web 伺服器 (IIS)

Windows Server 2016 中的 Web 服務器 (IIS) 角色提供安全、容易管理、模組化且可擴充的平臺，以可靠地裝載網站、服務和應用程式。 您可以使用 IIS，與網際網路、內部網路或外部網路上的使用者共用資訊。 IIS 是整合的 web 平臺，可將 IIS、ASP.NET、FTP 服務、PHP 和 Windows Communication Foundation (WCF) 。

如需詳細資訊，請參閱 [網頁伺服器 (IIS) 總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831725(v=ws.11))。