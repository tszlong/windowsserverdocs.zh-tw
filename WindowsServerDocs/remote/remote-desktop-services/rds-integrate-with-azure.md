---
title: RDS-與 Azure 服務整合
description: 了解如何整合到您的 Azure 部署中，與 Azure 的 RDS，在 RDS 部署。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/18/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.openlocfilehash: e582612496591356ed96b34522333d0e8bf34093
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879599"
---
# <a name="remote-desktop-services---integrating-with-azure-services"></a>遠端桌面服務-與 Azure 服務整合

Windows Server 2016 結合了功能強大的桌面及應用程式透過 Microsoft Azure 所提供的彈性、 可延展服務的遠端桌面服務安全傳遞。 您可以部署 RDS 與 Azure 服務，協助降低之內部部署伺服器的基礎結構維護成本、 提高穩定性，若要確保高可用性，請使用 multi-factor Authentication，來改善安全性使用 Azure 服務，並改善您使用現有的身分識別來存取 RDS 中的資源的使用者體驗

將 Azure 整合至您的遠端桌面部署中使用下列資訊：

- [了解如何使用 RDS 的多重要素驗證](/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway)
- [整合 Azure AD Domain Services 與 RDS 部署](rds-azure-adds.md)
- [使用 Azure AD 應用程式 Proxy 發佈遠端桌面](/azure/active-directory/application-proxy-publish-remote-desktop)

若要查看這些服務如何簡化您的遠端桌面部署的架構，請參閱[具有唯一的 Azure PaaS 角色的 RDS 架構](desktop-hosting-logical-architecture.md#rds-architectures-with-unique-azure-paas-roles)。