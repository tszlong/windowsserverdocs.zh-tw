---
title: 部署 802.1 X 有線與 Wireless 部署伺服器的憑證
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fda9b2b53d088cc389e98cf96fecd748419bc6e2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>部署 802.1 X 有線與 Wireless 部署伺服器的憑證

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此節目表伺服器的憑證部署至您遠端存取和網路原則 Server (NPS) 的基礎結構伺服器。   

本指南包含下列各節。  

-   [必要條件使用本指南](#bkmk_pre)  

-   [未提供哪些本指南](#bkmk_not)  

-   [技術概觀](#bkmk_tech)  

-   [伺服器的憑證部署概觀](Server-Certificate-Deployment-Overview.md)  

-   [伺服器的憑證部署計劃](Server-Certificate-Deployment-Planning.md)  

-   [伺服器的憑證部署](Server-Certificate-Deployment.md)  

### **<a name="digital-server-certificates"></a>數位伺服器的憑證**  
本指南使用 Active Directory 憑證 Services (AD CS) 自動註冊遠端存取和 NPS 基礎結構伺服器的憑證的指示操作。 AD CS 可讓您建置公用基礎結構 (PKI)，並提供您的組織公用密碼編譯、數位憑證及數位簽章的功能。  

當您使用數位伺服器的憑證來驗證您的網路上的電腦之間時，會提供的憑證：   

1. 透過加密機密性。  
2. 透過數位簽章完整性。  
3. 將憑證按鍵相關聯的電腦在網路上的電腦、使用者或裝置帳號驗證。  

### **<a name="server-types"></a>伺服器類型**  
使用此快速入門，您可以將伺服器的憑證部署至下列類型的伺服器。  
- 執行的伺服器遠端存取服務，DirectAccess 或標準 virtual 私人網路 (VPN) 伺服器、而成員的**RAS 及 IAS 伺服器]**群組。  
- 正在執行的網路原則 Server (NPS) 服務，伺服器的成員**RAS 及 IAS 伺服器]**群組。  

### **<a name="advantages-of-certificate-autoenrollment"></a>認證自動授權的優點**  
自動註冊伺服器的憑證，也稱為「自動註冊，提供下列優點。  

- AD CS 憑證授權單位自動註冊伺服器所有 NPS 及遠端存取伺服器的憑證。  
- 網域中的所有電腦就能都享受您所安裝的受信任的根憑證授權單位的 CA 憑證存放在每個成員網域的電腦上。 因為網域中的所有電腦標示為都信任的憑證所發行的授權。 這個信任可讓您驗證伺服器它們的身份彼此和參與安全通訊。  
- 以外重新整理群組原則，不需要手動重新設定的每個伺服器。  
- 每個伺服器的憑證包含伺服器驗證目的和 Client 驗證目的增強金鑰使用量 (EKU) 的擴充功能。  
- 擴充性。 部署本指南使用您的企業根 CA 之後, 您可以透過新增企業附屬 Ca 展開公用基礎結構 (PKI)。  
- 管理性。 您可以管理 AD CS，請使用 AD CS 主機或使用 Windows PowerShell 命令和指令碼。  
- 簡單。 指定的伺服器，使用 Active Directory 群組帳號和群組成員資格註冊伺服器的憑證。   
- 當您部署伺服器的憑證時，憑證依據您使用本文中的指示來設定範本。 這表示您可以自訂不同的憑證範本特定伺服器類型，或您可以使用相同的範本所有伺服器的憑證，以您想要發行。  

## <a name="bkmk_pre"></a>必要條件使用本指南  

本指南使用中 Windows Server 2016 AD CS 與 Web 伺服器 (IIS) 伺服器角色部署伺服器的憑證的方式指示。 以下是此節目表中執行的程序的必要條件。  

- 您必須部署核心網路使用「Windows Server 2016 核心網路快速入門，或是您必須已經核心網路指南安裝，您網路上正確運作中所提供的技術。 這些技術包括 TCP/IP v4，DHCP、Active Directory Domain Services (AD DS)，DNS 及 NPS。  
>[!NOTE]
>Windows Server 2016 核心網路指南可在 Windows Server 2016 技術媒體櫃。 如需詳細資訊，請查看[核心網路指南](../../../core-network-guide/Core-Network-Guide.md)。

- 您必須讀取以確保您的準備此部署部署執行之前，先本指南計劃一節。  
- 您必須執行步驟本指南則會顯示順序。 不要加大和部署 CA 您無須執行導致部署伺服器或您的部署步驟將會失敗。  
- 您必須準備好要部署兩部新伺服器您網路中的上一個伺服器時，您將會安裝 AD CS 企業根加拿大、時，您將會安裝網頁伺服器 (IIS)，讓您 CA 可以發行網頁伺服器的憑證撤銷清單 (CRL) 伺服器。   

>[!NOTE]  
>您已準備好將靜態 IP 位址指派給本指南，以及至於名稱根據您的組織命名規格的電腦與您部署的網路與 AD CS 伺服器。 此外，您必須將電腦加入您的網域。  

## <a name="bkmk_not"></a>未提供哪些本指南  
本指南設計和部署公用基礎結構 (PKI)，請使用 AD CS 不提供完整的指示操作。 建議您檢視的文件 AD CS 和部署本文中的技術之前 PKI 設計文件。   

## <a name="bkmk_tech"></a>技術概觀  
以下是 AD CS 與 Web 伺服器 (IIS) 技術概觀。  

### <a name="active-directory-certificate-services"></a>Active Directory 憑證服務  
在 Windows Server 2016 AD CS 提供自訂建立及管理 x.509 軟體安全性系統的運用公用主要技術中所使用的服務。 組織可用來提升安全性繫結至對應公用按鍵的人員、裝置或服務的身分 AD CS。 AD CS 也包含了功能，可讓您管理憑證註冊與撤銷各種不同的延展性環境中。  

如需詳細資訊，請查看[Active Directory 憑證服務概觀](https://technet.microsoft.com/library/hh831740.aspx)和[公開鍵基礎結構設計指導方針](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)。  

### <a name="web-server-iis"></a>網頁伺服器 (IIS)  

在 Windows Server 2016 網頁伺服器 (IIS) 角色提供安全，輕鬆管理、模組，且最具擴充性的平台會可靠地裝載的網站、服務和應用程式。 使用 IIS，您可以分享網際網路、 內部網路，或外部網路使用者的資訊。 IIS 是整合 IIS、 ASP.NET、 FTP 服務、 PHP 及 Windows 通訊基本知識 (WCF) 的整合的 web 平台。  

如需詳細資訊，請查看[網頁伺服器 (IIS) 概觀](https://technet.microsoft.com/library/hh831725.aspx)。  
