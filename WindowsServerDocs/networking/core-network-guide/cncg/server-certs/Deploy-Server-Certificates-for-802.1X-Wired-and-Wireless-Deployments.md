---
title: 部署 802.1 X 有線和無線部署的伺服器憑證
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0dce886555167ad651704045120fb92eff0dcea1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356183"
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>部署 802.1 X 有線和無線部署的伺服器憑證

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本指南，將伺服器憑證部署至您的遠端存取和網路原則伺服器（NPS）基礎結構伺服器。   

本指南涵蓋下列各節。  

-   [使用本指南的必要條件](#bkmk_pre)  

-   [本指南中未提供的內容](#bkmk_not)  

-   [技術概覽](#bkmk_tech)  

-   [伺服器憑證部署總覽](Server-Certificate-Deployment-Overview.md)  

-   [伺服器憑證部署規劃](Server-Certificate-Deployment-Planning.md)  

-   [伺服器憑證部署](Server-Certificate-Deployment.md)  

### <a name="digital-server-certificates"></a>**數位伺服器憑證**  
本指南提供使用 Active Directory 憑證服務（AD CS）將憑證自動註冊至遠端存取和 NPS 基礎結構伺服器的指示。 AD CS 可讓您建立公開金鑰基礎結構（PKI），並為您的組織提供公開金鑰加密、數位憑證和數位簽章功能。  

當您在網路上的電腦之間使用數位伺服器憑證進行驗證時，憑證會提供：   

1. 透過加密的機密性。  
2. 透過數位簽章的完整性。  
3. 藉由將憑證金鑰與電腦網路上的電腦、使用者或裝置帳戶產生關聯，進行驗證。  

### <a name="server-types"></a>**伺服器類型**  
藉由使用本指南，您可以將伺服器憑證部署到下列類型的伺服器。  
- 執行遠端存取服務的伺服器，也就是 DirectAccess 或標準虛擬私人網路（VPN）伺服器，而且是**RAS 和 資訊存取伺服器**群組的成員。  
- 執行網路原則伺服器（NPS）服務的伺服器，其為**RAS 和 資訊存取伺服器**群組的成員。  

### <a name="advantages-of-certificate-autoenrollment"></a>**憑證自動註冊的優點**  
自動註冊伺服器憑證（也稱為自動註冊）可提供下列優點。  

- AD CS 憑證授權單位單位（CA）會自動向您的所有 NPS 和遠端存取服務器註冊伺服器憑證。  
- 網域中的所有電腦都會自動接收您的 CA 憑證，這會安裝在每一部網域成員電腦上的「信任的根憑證授權」存放區中。 因此，網域中的所有電腦都信任 CA 所簽發的憑證。 此信任可讓您的驗證服務器向彼此證明其身分識別，並參與安全通訊。  
- 除了重新整理群組原則，每個伺服器的手動重新設定都不是必要的。  
- 每個伺服器憑證都包含「伺服器驗證」目的和「用戶端驗證」目的，以增強金鑰使用方法（EKU）延伸。  
- 延展性。 使用本指南部署您的企業根 CA 之後，您可以藉由新增企業次級 Ca 來擴充您的公開金鑰基礎結構（PKI）。  
- 管理性。 您可以使用 AD CS 主控台，或使用 Windows PowerShell 命令和腳本來管理 AD CS。  
- 簡單。 您可以使用 Active Directory 群組帳戶和群組成員資格，指定用來註冊伺服器憑證的伺服器。   
- 當您部署伺服器憑證時，憑證是以您使用本指南中的指示進行設定的範本為基礎。 這表示您可以針對特定伺服器類型自訂不同的憑證範本，也可以針對您要發出的所有伺服器憑證使用相同的範本。  

## <a name="bkmk_pre"></a>使用本指南的必要條件  

本指南提供如何使用 AD CS 和 Windows Server 2016 中的網頁伺服器（IIS）伺服器角色來部署伺服器憑證的指示。 以下是執行本指南中程式的必要條件。  

- 您必須使用 Windows Server 2016 核心網路指南部署核心網路，或者您必須已經安裝核心網路指南中所提供的技術，並在您的網路上正常運作。 這些技術包括 TCP/IP v4、DHCP、Active Directory Domain Services （AD DS）、DNS 和 NPS。  
  >[!NOTE]
  >Windows server 2016 核心網路指南可在 Windows Server 2016 技術文件庫中取得。 如需詳細資訊，請參閱[核心網路指南](../../../core-network-guide/Core-Network-Guide.md)。

- 您必須閱讀本指南的規劃一節，以確保您在執行部署之前已準備好進行這項部署。  
- 您必須依照其呈現順序來執行本指南中的步驟。 請不要繼續進行並部署您的 CA，而不需執行導致部署伺服器的步驟，否則您的部署將會失敗。  
- 您必須準備好在您的網路上部署兩部新的伺服器-一台伺服器，您會將 AD CS 安裝為企業根 CA，而一個伺服器則會安裝網頁伺服器（IIS），讓您的 CA 可以將憑證撤銷清單（CRL）發佈到 Web se伺服器.   

>[!NOTE]  
>您已準備好要將靜態 IP 位址指派給您使用本指南部署的 Web 和 AD CS 伺服器，以及根據組織的命名慣例來命名電腦。 此外，您必須將電腦加入您的網域。  

## <a name="bkmk_not"></a>本指南未提供的內容  
本指南不提供使用 AD CS 設計和部署公開金鑰基礎結構（PKI）的完整指示。 建議您先參閱 AD CS 檔和 PKI 設計檔，再部署本指南中的技術。   

## <a name="bkmk_tech"></a>技術概覽  
以下是 AD CS 和網頁伺服器（IIS）的技術概覽。  

### <a name="active-directory-certificate-services"></a>Active Directory 憑證服務  
Windows Server 2016 中的 AD CS 提供可自訂的服務，用來建立和管理使用公開金鑰技術的軟體安全性系統中所用的 x.509 憑證。 組織可以藉由將個人、裝置或服務的身分識別系結至對應的公開金鑰，來使用 AD CS 來加強安全性。 AD CS 還包含功能，可讓您管理各種可擴充環境中的憑證註冊和撤銷。  

如需詳細資訊，請參閱[Active Directory 憑證服務總覽](https://technet.microsoft.com/library/hh831740.aspx)和[公開金鑰基礎結構設計指導](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)方針。  

### <a name="web-server-iis"></a>Web 伺服器 (IIS)  

Windows Server 2016 中的網頁伺服器（IIS）角色提供安全、容易管理、模組化且可擴充的平臺，讓您能夠可靠地裝載網站、服務和應用程式。 使用 IIS 時，您可以與網際網路、內部網路或外部網路上的使用者共用資訊。 IIS 是整合 IIS、ASP.NET、FTP 服務、PHP 和 Windows Communication Foundation （WCF）的統一 web 平臺。  

如需詳細資訊，請參閱[網頁伺服器（IIS）總覽](https://technet.microsoft.com/library/hh831725.aspx)。  
