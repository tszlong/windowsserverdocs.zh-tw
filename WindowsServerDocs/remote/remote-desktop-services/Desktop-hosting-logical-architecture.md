---
title: 遠端桌面服務架構
description: RDS 的架構圖
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 02/10/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f73bb0a-ce98-48a4-9d9f-cf7438936ca1
author: lizap
manager: dongill
ms.openlocfilehash: ba597318cdaf1d4659a72905eeb4e252c9020e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887869"
---
# <a name="remote-desktop-services-architecture"></a>遠端桌面服務架構

>適用於：Windows Server （半年通道），Windows Server 2016

以下是部署遠端桌面服務來裝載 Windows 應用程式和使用者的桌上型電腦的各種設定。

>[!NOTE]
> 下列架構圖表顯示在 Azure 中使用 RDS。 不過，您可以將遠端桌面服務在內部部署和其他雲端上。 這些圖表主要是為了說明解 RDS 角色共置，並使用其他服務。

## <a name="standard-rds-deployment-architectures"></a>標準 RDS 部署架構

遠端桌面服務有兩個標準的架構：
-   基本部署 – 這包含要建立完整有效的 RDS 環境的伺服器數目下限
-   高可用性部署 – 這包含所有必要的元件，將您 RDS 環境的最高保證執行時間

### <a name="basic-deployment"></a>基本部署

![基本 RDS 部署](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>高可用性部署

![高度可用的 RDS 部署](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>使用唯一的 Azure PaaS 角色的 RDS 架構

雖然標準 RDS 部署架構符合大部分的情況下，Azure 會持續將心力投注在第一方 PaaS 解決方案的推動客戶價值。 以下是一些顯示它們如何兼顧 RDS 的架構

### <a name="rds-deployment-with-azure-ad-domain-services"></a>使用 Azure AD Domain Services 的 RDS 部署

兩個以上的標準架構圖表以在傳統 Active Directory (AD) 的 Windows Server VM 上部署。 不過，如果不有傳統的 AD，且只能使用 Azure AD 租用戶 — 透過像是 Office365 服務，但仍想要運用 RDS，您可以使用[Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)建立在 Azure IaaS 中的完全受控的網域使用相同的使用者存在於 Azure AD 租用戶中的環境。 這會移除以手動方式同步處理使用者和管理多部虛擬機器的複雜性。 Azure AD 網域服務可以在其中一個部署中運作： 基本或高可用性。

![Azure AD 與 RDS 部署](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>使用 Azure AD 應用程式 Proxy 的 RDS 部署

兩個以上的標準架構圖表會將 RD Web/閘道伺服器作為 RDS 系統的網際網路對向進入點。 對於某些環境中，系統管理員會想要從周邊網路中移除自己的伺服器，並改用也提供額外的安全性，透過反向 proxy 技術的技術。 [Azure AD 應用程式 Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) PaaS 角色完全適用於在此案例中。

如需支援的設定和如何建立此設定，請參閱如何[使用 Azure AD 應用程式 Proxy 發佈遠端桌面](/azure/active-directory/application-proxy-publish-remote-desktop)。

![使用 Azure AD 應用程式 Proxy 的 RDS](./media/aadappproxy-rds.png)
