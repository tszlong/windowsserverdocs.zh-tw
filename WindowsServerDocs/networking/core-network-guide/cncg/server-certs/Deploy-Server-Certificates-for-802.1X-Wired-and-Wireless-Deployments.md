---
title: 部署 802.1 X 有線和無線部署的伺服器憑證
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 769a4165cfd82056a904c79c41e96fb666d05e43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842269"
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>部署 802.1 X 有線和無線部署的伺服器憑證

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本指南將伺服器憑證部署至您的遠端存取和網路原則伺服器 (NPS) 基礎結構伺服器。   

本指南涵蓋下列各節。  

-   [使用本指南的必要條件](#bkmk_pre)  

-   [本指南中未提供的內容](#bkmk_not)  

-   [技術概觀](#bkmk_tech)  

-   [伺服器憑證部署概觀](Server-Certificate-Deployment-Overview.md)  

-   [伺服器憑證部署規劃](Server-Certificate-Deployment-Planning.md)  

-   [伺服器憑證部署](Server-Certificate-Deployment.md)  

### <a name="digital-server-certificates"></a>**數位伺服器憑證**  
本指南提供使用 Active Directory 憑證服務 (AD CS) 自動註冊憑證給遠端存取和 NPS 基礎結構伺服器的指示。 AD CS 可以讓您建置公開金鑰基礎結構 (PKI)，並為您的組織提供公開金鑰密碼編譯、 數位憑證以及數位簽章功能。  

當您使用您網路上電腦之間進行驗證的數位伺服器憑證時，憑證提供：   

1. 透過加密的機密性。  
2. 透過數位簽章的完整性。  
3. 與電腦網路上的電腦、 使用者或裝置帳戶產生關聯的憑證金鑰的驗證。  

### <a name="server-types"></a>**伺服器類型**  
藉由使用本指南，您可以部署到下列伺服器類型的伺服器憑證。  
- 伺服器執行遠端存取服務，為 DirectAccess 或標準的 「 虛擬私人網路 (VPN) 伺服器，以及隸屬**RAS 與 IAS 伺服器**群組。  
- 伺服器正在執行網路原則伺服器 (NPS) 服務，屬於**RAS 與 IAS 伺服器**群組。  

### <a name="advantages-of-certificate-autoenrollment"></a>**憑證自動註冊的優點**  
自動註冊的伺服器憑證，也稱為自動註冊，可提供下列優點。  

- AD CS 憑證授權單位 (CA) 自動註冊 NPS 及遠端存取伺服器的所有伺服器憑證。  
- 網域中的所有電腦會自動都收到您已安裝在受信任的根憑證授權單位的 CA 憑證儲存在每一部網域成員電腦上。 因為這個緣故，在網域中的所有電腦會都信任您的 CA 所簽發的憑證。 此信任可讓您驗證伺服器證明其身分識別彼此和參與通訊安全。  
- 不重新整理群組原則，則不需要手動重新設定的每一部伺服器。  
- 每個伺服器憑證包含伺服器驗證目的和用戶端驗證目的，在 增強金鑰使用方法 (EKU) 延伸模組。  
- 延展性。 部署您的企業根 CA，使用此指南之後, 您可以展開公開金鑰基礎結構 (PKI) 藉由新增企業次級 Ca。  
- 管理性。 您可以使用 AD CS 主控台或透過 Windows PowerShell 命令與指令碼來管理 AD CS。  
- 簡單。 您指定的伺服器，使用 Active Directory 群組帳戶和群組成員資格來註冊伺服器憑證。   
- 當您部署伺服器憑證時，憑證會被您使用本指南中的指示來設定範本為基礎。 這表示您可以自訂特定伺服器類型的不同的憑證範本，或您可以使用相同的範本，您想要發出的所有伺服器憑證。  

## <a name="bkmk_pre"></a>使用本指南的必要條件  

本指南提供如何部署 Windows Server 2016 中使用 AD CS 和網頁伺服器 (IIS) 伺服器角色的伺服器憑證的指示。 以下是本指南中執行的程序的必要條件。  

- 您必須部署核心網路使用 Windows Server 2016 核心網路指南，或您必須已安裝且您的網路上運作正常，才會進行核心網路指南中提供的技術。 這些技術包括 TCP/IP v4，DHCP，Active Directory 網域服務 (AD DS)、 DNS 及 NPS。  
>[!NOTE]
>Windows Server 2016 核心網路指南位於 Windows Server 2016 技術文件庫。 如需詳細資訊，請參閱 <<c0> [ 核心網路指南](../../../core-network-guide/Core-Network-Guide.md)。

- 您必須閱讀本指南，以確保您已準備此部署在執行部署之前規劃一節。  
- 您必須在本指南中在其中出現的順序執行步驟。 請勿往前跳並部署您的 CA，沒有執行到部署伺服器或您的部署的步驟將會失敗。  
- 您必須準備好部署兩個新的伺服器網路-上一部伺服器，您將在其中安裝 AD CS 做為企業根 CA，並在其中您會安裝網頁伺服器 (IIS)，讓您的 CA 可以發行至 Web se 的憑證撤銷清單 (CRL) 的一部伺服器rver。   

>[!NOTE]  
>您已準備好將靜態 IP 位址指派給您使用本指南中，以及有關命名的電腦，根據您組織的命名慣例來部署 Web 和 AD CS 伺服器。 此外，您必須將電腦加入您的網域。  

## <a name="bkmk_not"></a>本指南未提供的內容  
本指南不提供完整的指示，來設計和使用 AD CS 部署公開金鑰基礎結構 (PKI)。 建議您檢閱 AD CS 文件和 PKI 設計文件，再部署本指南中的技術。   

## <a name="bkmk_tech"></a>技術概觀  
以下是 AD CS 和網頁伺服器 (IIS) 的技術概觀。  

### <a name="active-directory-certificate-services"></a>Active Directory 憑證服務  
Windows Server 2016 中的 AD CS 提供建立和管理採用公開金鑰技術的軟體安全性系統中使用的 X.509 憑證的自訂服務。 組織可以使用 AD CS 來加強安全性繫結至對應的公開金鑰的個人、 裝置或服務的身分識別。 AD CS 也包含可讓您管理各種彈性環境中的憑證註冊與撤銷的功能。  

如需詳細資訊，請參閱 < [Active Directory 憑證服務概觀](https://technet.microsoft.com/library/hh831740.aspx)並[公用金鑰基礎結構設計指引](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)。  

### <a name="web-server-iis"></a>Web 伺服器 (IIS)  

Windows Server 2016 中的網頁伺服器 (IIS) 角色提供安全、 容易管理、 模組化且可擴充的平台能夠可靠地主控網站、 服務和應用程式。 使用 IIS 時，您可以共用網際網路、 內部或外部網路與使用者資訊。 IIS 是一個整合 IIS、 ASP.NET、 FTP 服務、 PHP 和 Windows Communication Foundation (WCF) 的統合式的 web 平台。  

如需詳細資訊，請參閱 <<c0> [ 網頁伺服器 (IIS) 概觀](https://technet.microsoft.com/library/hh831725.aspx)。  
