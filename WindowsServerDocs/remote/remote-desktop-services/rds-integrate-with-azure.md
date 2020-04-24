---
title: RDS - 與 Azure 服務整合
description: 了解如何將 RDS 整合到您的 Azure 部署中，以及如何將 Azure 整合到 RDS 部署中。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/18/2017
ms.topic: article
author: lizap
ms.openlocfilehash: 2c792a22608fe0e7ae826b27d6917202037559e4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855071"
---
# <a name="remote-desktop-services---integrating-with-azure-services"></a>遠端桌面服務 - 與 Azure 服務整合

Windows Server 2016 透過遠端桌面服務和 Microsoft Azure 提供的彈性、可擴充服務，結合了功能強大的桌面和應用程式安全傳遞。 您可以部署 RDS 與 Azure 服務，以利降低內部部署伺服器的基礎結構維護成本、使用 Azure 服務確保高可用性以提高穩定性、使用多重要素驗證以改善安全性，以及使用現有的身分識別存取 RDS 中的資源以改進使用者體驗。

請使用下列資訊，將 Azure 整合到您的遠端桌面部署中：

- [深入了解如何搭配 RDS 使用多重要素驗證](/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway)
- [將 Azure AD Domain Services 與您的 RDS 部署整合](rds-azure-adds.md)
- [使用 Azure AD 應用程式 Proxy 發佈遠端桌面](/azure/active-directory/application-proxy-publish-remote-desktop)

若要查看這些服務如何簡化您的遠端桌面部署架構，請參閱[具有唯一 Azure PaaS 角色的 RDS 架構](desktop-hosting-logical-architecture.md#rds-architectures-with-unique-azure-paas-roles)。