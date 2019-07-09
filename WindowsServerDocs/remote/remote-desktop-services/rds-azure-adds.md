---
title: Azure AD Domain Services 和遠端桌面服務
description: 了解如何將 Azure AD Domain Services 與您的 RDS 部署整合。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/02/2017
ms.tgt_pltfrm: na
ms.topic: article
author: christianmontoya
ms.localizationpriority: medium
ms.openlocfilehash: 8b1baf642ffa3c8e8a0a2cfc70d2f49b58f208b3
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "66446578"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>將 Azure AD Domain Services 與您的 RDS 部署整合

您可以使用遠端桌面服務部署中的 [Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) 取代 Windows Server Active Directory。 Azure AD DS 可讓您在傳統的 Windows 工作負載中使用您現有的 Azure AD 身分識別。

使用 Azure AD DS，您可以： 
- 針對產生自雲端的組織，建立具有本機網域的 Azure 環境。 
- 使用內部部署和線上環境中所使用的相同身分識別，建立隔離的 Azure 環境，而不需要建立站台對站台 VPN 或 ExpressRoute。 

當您將 Azure AD DS 整合到您的遠端桌面部署完畢時，您的架構會看起來像這樣：

![顯示具有 Azure AD DS 之 RDS 的架構圖](media/aadds-rds.png)

若要了解此架構與其他 RDS 部署案例的比較，請參閱[遠端桌面服務架構](desktop-hosting-logical-architecture.md)。

若要深入了解 Azure AD DS，請參閱 [Azure AD DS 概觀](/azure/active-directory-domain-services/active-directory-ds-overview)以及[如何決定 Azure AD DS 是否適合您的使用案例](/azure/active-directory-domain-services/active-directory-ds-comparison)。

使用下列資訊部署具有 RDS 的 Azure AD DS。

## <a name="prerequisites"></a>必要條件

在您可以將 Azure AD 中的身分識別用於 RDS 部署之前，請[設定 Azure AD，以儲存您使用者身分識別的雜湊密碼](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync)。 產生自雲端的組織不需要在其目錄中進行任何額外的變更，不過，內部部署組織必須允許同步處理密碼雜湊並儲存在 Azure AD 中，這在某些組織中可能是不允許的。 使用者必須在進行這項設定變更後重設其密碼。

## <a name="deploy-azure-ad-ds-and-rds"></a>部署 Azure AD DS 和 RDS 
使用下列步驟部署 Azure AD DS 和 RDS。

1. 啟用 [Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started)。 請注意，連結的文章會進行下列操作：
   - 逐步解說如何建立適當的 Azure AD 群組以進行網域管理。
   - 強調您可能必須強制使用者變更其密碼，讓他們的帳戶能夠搭配 Azure AD DS 使用。
   
2. 設定 RDS。 您可以使用 Azure 範本，或手動部署 RDS。
   - 使用 [現有的 AD 範本](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)。 請務必自訂下列內容：
   
     - **設定**
       - **資源群組**：使用您想要建立 RDS 資源所在的資源群組。
         > [!NOTE] 
         > 現在，這必須是 Azure Resource Manager 虛擬網路所在的相同的資源群組。

       - **DNS 標籤首碼**：輸入您希望使用者用來存取 RD Web 的 URL。
       - **AD 網域名稱**：輸入您 Azure AD 執行個體的完整名稱，例如 "contoso.onmicrosoft.com" 或 "contoso.com"。
       - **AD Vnet 名稱**和**AD 子網路名稱**：輸入您在建立 Azure Resource Manager 虛擬網路時使用的相同值。 這是 RDS 資源要連線的子網路。
       - **系統管理員使用者名稱**和**系統管理員密碼**：輸入 Azure AD 中 **AAD DC 系統管理員**群組成員之系統管理員使用者的認證。
   
     - **範本**
        - 移除 **dnsServers** 的所有屬性：從 Azure 快速入門範本頁面中選取 [編輯範本]  之後，搜尋 "dnsServers"，然後移除該屬性。 

           例如，在移除 **dnsServers** 屬性之前：
      
           ![具有 dnsSettings 屬性的 Azure 快速入門範本](media/rds-remove-dnssettings-before.png)

           此外，以下是移除該屬性之後的相同檔案：

           ![已移除 dnsSettings 屬性的 Azure 快速入門範本](media/rds-remove-dnssettings-after.png)
   
   - [手動部署 RDS](rds-deploy-infrastructure.md)。 

