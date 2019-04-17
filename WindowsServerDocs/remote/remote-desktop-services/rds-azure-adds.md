---
title: Azure AD 網域服務與遠端桌面服務
description: 了解如何將 Azure AD 網域服務整合到您的 RDS 部署。
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
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1614512"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>整合使用 RDS 部署 Azure AD 網域服務

您可以使用[Azure AD 網域服務](/azure/active-directory-domain-services/active-directory-ds-overview)(Azure AD DS) 以 Windows Server Active Directory 取代在遠端桌面服務部署中。 Azure AD DS 可讓您使用您現有的 Azure AD 身分識別使用傳統 Windows 工作負載。

Azure AD DS 您可以進行下列作業： 
- 使用出生中-雲端組織的本機網域中建立 Azure 環境。 
- 建立用於您的內部部署和線上環境，而不需要建立的網站 VPN 或 ExpressRoute 同一個身分識別隔離 Azure 環境。 

當您完成將 Azure AD DS 整合到您的遠端桌面部署您的架構看起來會類似：

![顯示與 Azure AD DS 的 RDS 架構圖表](media/aadds-rds.png)

若要查看此架構與其他 RDS 部署案例的會比較，請參閱[遠端桌面服務架構](desktop-hosting-logical-architecture.md)。

若要取得 Azure AD DS 的改善了解，請查看[Azure AD DS 概觀 （英文）](/azure/active-directory-domain-services/active-directory-ds-overview)以及[如何決定 Azure AD DS 是否適合您的使用案例](/azure/active-directory-domain-services/active-directory-ds-comparison)。

使用下列的資訊來部署 Azure AD DS 與 rds。

## <a name="prerequisites"></a>先決條件

之前您可以將您的身分識別中使用 RDS 部署、[設定以儲存您的使用者身分識別密碼雜湊 Azure AD](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync)的 Azure AD。 出生的-雲端組織不需要在其目錄; 進行任何其他變更不過，在內部部署組織需要允許密碼雜湊同步處理並儲存在可能不允許有些組織中的 Azure AD。 使用者必須進行這項設定變更之後重設其密碼。

## <a name="deploy-azure-ad-ds-and-rds"></a>部署 Azure AD DS 與 RDS 
使用下列步驟來部署 Azure AD DS 與 rds。

1. 啟用[Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started)。 請注意下列連結的文章會下列事項：
   - 逐步解說建立適當的網域管理 Azure AD 群組。
   - 反白顯示當您可能必須強制使用者讓其帳戶可搭配 Azure AD DS 變更其密碼。
   
2. 設定 rds。 您可以使用 Azure 範本或手動部署 RDS。
   - 使用[現有 AD 範本](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)。 請確定自訂下列：
   
      - **設定**
         - **資源群組**： 使用資源群組您要建立的 RDS 資源。
         > [!NOTE] 
         > 向右現在這必須是相同的資源群組所在 Azure 資源管理員虛擬網路。

         - **Dns 標籤前置詞**： 輸入 URL 您想要用來存取 RD 網頁的使用者。
         - **Ad 網域名稱**： 輸入您 Azure AD 的執行個體，例如"contoso.onmicrosoft.com"或"contoso.com"的完整名稱。
         - **Ad Vnet 名稱**和**Ad 子網路名稱**： 輸入您建立 Azure 資源管理員虛擬網路時所使用的值相同。 這是要連線的 RDS 資源之子網路。
         - **系統管理員使用者名稱**及**系統密碼**： 輸入處於 Azure AD 的**AAD DC 系統管理員**群組的成員管理使用者的認證。
   
      - **範本**
         - 移除**dnsServers**的所有屬性： 選取之後**編輯範本**從 Azure 快速入門範本] 頁面上，搜尋"dnsServers"並都移除屬性。 

            例如，然後再移除**dnsServers**屬性：
      
            ![Azure 快速入門範本使用 dnsSettings 屬性](media/rds-remove-dnssettings-before.png)

            而以下是同一個檔案後移除屬性：

            ![使用 dnsSettings 屬性中移除的 azure 快速入門範本](media/rds-remove-dnssettings-after.png)
   
   - [部署 RDS 手動](rds-deploy-infrastructure.md)。 

