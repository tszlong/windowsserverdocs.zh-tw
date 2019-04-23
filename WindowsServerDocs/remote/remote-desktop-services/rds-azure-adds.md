---
title: Azure AD 網域服務和遠端桌面服務
description: 了解如何將 Azure AD Domain Services 整合到您的 RDS 部署。
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
ms.openlocfilehash: e60cf70f1f91ad87046bedf024fe9afc459075b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860509"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>將 Azure AD Domain Services 與 RDS 部署整合

您可以使用[Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) 中遠端桌面服務部署 Windows Server Active Directory 取代。 Azure AD DS 可讓您使用您現有的 Azure AD 身分識別，使用傳統的 Windows 工作負載。

您可以使用 Azure AD DS: 
- 建立本機網域的誕生---雲端中組織的 Azure 環境。 
- 建立隔離的 Azure 環境以用於您的內部部署和線上環境中，而不需要建立站對站 VPN 或 ExpressRoute 的相同身分識別。 

當您完成將 Azure AD DS 整合到您的遠端桌面部署時，您的架構會看起來像這樣：

![顯示 RDS 與 Azure AD DS 架構圖](media/aadds-rds.png)

若要了解此架構與其他 RDS 部署案例，請參閱[遠端桌面服務架構](desktop-hosting-logical-architecture.md)。

若要深入了解 Azure AD DS，請參閱[Azure AD DS 概觀](/azure/active-directory-domain-services/active-directory-ds-overview)並[如何決定 Azure AD DS 是否適合您的使用案例](/azure/active-directory-domain-services/active-directory-ds-comparison)。

使用下列資訊來部署 RDS 與 Azure AD DS

## <a name="prerequisites"></a>先決條件

您可以從 Azure AD，以使用在 RDS 部署中，將您的身分識別才能[設定 Azure AD，以儲存您的使用者身分識別的雜湊的密碼](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync)。 產自---雲端中的組織不需要進行任何額外的變更，在他們的目錄;不過，在內部部署組織必須允許密碼雜湊同步處理並儲存在 Azure AD 中，可能不允許某些組織中。 使用者必須進行這項設定變更後重設其密碼。

## <a name="deploy-azure-ad-ds-and-rds"></a>將 Azure AD DS 和 RDS 部署 
使用下列步驟來部署 Azure AD DS 和 rds。

1. 啟用[Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started)。 請注意，連結的文章會下列作業：
   - 逐步解說建立適當網域系統管理的 Azure AD 群組。
   - 您可能要強制使用者變更其密碼，讓他們的帳戶能夠使用 Azure AD DS 時，反白顯示。
   
2. 設定 RDS 您可以使用 Azure 範本，或以手動方式部署 RDS。
   - 使用[現有 AD 範本](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)。 請確定下列自訂：
   
      - **設定**
         - **资源组**：使用您想要用來建立 RDS 資源的資源群組。
         > [!NOTE] 
         > 現在此值必須是 Azure resource manager 虛擬網路所在的相同的資源群組的權限。

         - **Dns 標籤首碼**:輸入的 URL 想要用來存取 RD Web 的使用者。
         - **Ad 網域名稱**:輸入您 Azure AD 執行個體，例如"contoso.onmicrosoft.com"或"contoso.com"的完整名稱。
         - **Ad Vnet-name**並**Ad 子網路名稱**:輸入您在建立 Azure resource manager 虛擬網路時使用的相同值。 這是 RDS 資源要連線的子網路。
         - **系統管理員使用者名稱**並**系統管理員密碼**:輸入成員的系統管理員使用者的認證**AAD DC 系統管理員**群組在 Azure AD 中。
   
      - **範本**
         - 移除的所有屬性**dnsServers**： 在選取之後**編輯範本**從 Azure 快速入門範本 頁面中，搜尋"dnsServers 」，並都移除屬性。 

            比方說，然後再移除**dnsServers**屬性：
      
            ![Azure 快速入門範本具有 dnsSettings 屬性](media/rds-remove-dnssettings-before.png)

            而以下是相同的檔案之後移除屬性,：

            ![Azure 快速入門範本，以移除 dnsSettings 屬性](media/rds-remove-dnssettings-after.png)
   
   - [手動部署 RDS](rds-deploy-infrastructure.md)。 

