---
title: 遠端桌面服務架構
description: RDS 的架構圖表
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 02/10/2017
ms.topic: article
ms.assetid: 7f73bb0a-ce98-48a4-9d9f-cf7438936ca1
author: lizap
manager: dongill
ms.openlocfilehash: 2635302c79f5bfb8ca446f78d543e19656644102
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966710"
---
# <a name="remote-desktop-services-architecture"></a>遠端桌面服務架構

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

以下是部署遠端桌面服務來為終端使用者代管 Windows 應用程式和桌面的各種設定。

>[!NOTE]
> 底下的架構圖表示範如何在 Azure 中使用 RDS。 不過，您可以在內部部署和其他雲端上部署遠端桌面服務。 這些圖表主要是為了說明 RDS 角色如何共置及使用其他服務。

## <a name="standard-rds-deployment-architectures"></a>標準 RDS 部署架構

遠端桌面服務具有兩種標準架構：
-    基本部署 - 這包含為建立完全有效 RDS 環境的最少伺服器數目
-    高可用性部署 - 這包含為保證 RDS 環境有最高執行時間的所有必要元件

### <a name="basic-deployment"></a>基本部署

![基本 RDS 部署](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>高可用性部署

![高可用性 RDS 部署](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>具有唯一 Azure PaaS 角色的 RDS 架構

雖然標準 RDS 部署架構適合大部分的情況，但 Azure 會持續將心力投注在第一方 PaaS 解決方案來提高客戶價值。 以下是示範它們如何與 RDS 結合的一些架構。

### <a name="rds-deployment-with-azure-ad-domain-services"></a>使用 Azure AD Domain Services 的 RDS 部署

上述兩個標準架構圖表是以部署在 Windows Server VM 上的傳統 Active Directory (AD) 為基礎。 不過，如果您沒有傳統 AD 而只有 Azure AD 租用戶 (透過 Office365 之類的服務)，但仍想要利用 RDS，您可以使用 [Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) 在 Azure IaaS 環境中建立完全受控的網域，以使用存在於 Azure AD 租用戶中的相同使用者。 這可避免以手動方式同步使用者和管理更多虛擬機器的複雜作業。 Azure AD Domain Services 可以在下列任一部署中運作：基本或高可用性。

![Azure AD 和 RDS 部署](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>使用 Azure AD 應用程式 Proxy 的 RDS 部署

上述兩個標準架構圖表使用 RD 網頁/閘道伺服器作為 RDS 系統的網際網路對向進入點。 在某些環境中，系統管理員會想要從周邊移除自己的伺服器，並改用會同時透過反向 Proxy 技術提供額外安全性的技術。 [Azure AD 應用程式 Proxy](/azure/active-directory/active-directory-application-proxy-get-started) PaaS 角色非常適合此情況。

若要了解支援的設定及如何建立此設定，請參閱[使用 Azure AD 應用程式 Proxy 發佈遠端桌面](/azure/active-directory/application-proxy-publish-remote-desktop)。

![RDS 搭配 Azure AD 應用程式 Proxy](./media/aadappproxy-rds.png)
