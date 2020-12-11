---
description: 深入瞭解：部署同盟伺服器 proxy
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: 在 Windows Server 2012 R2 的 AD FS 中部署同盟伺服器 proxy
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 6554cd206c459d5b0eacb8bf375d5fcd132c8530
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048886"
---
# <a name="deploying-federation-server-proxies"></a>部署同盟伺服器 Proxy

在 \( \) Windows Server 2012 R2 的 Active Directory 同盟服務 AD FS 中，同盟伺服器 proxy 的角色是由新的遠端存取角色服務（稱為 Web 應用程式 proxy）所處理。 若要從公司網路外部啟用 AD FS 的協助工具，這是在舊版 AD FS 中部署同盟伺服器 proxy 的目的，例如 AD FS 2.0 和 Windows Server 2012 中的 AD FS，您可以在 Windows Server 2012 R2 中部署 AD FS 的一或多個 web 應用程式 proxy。

在 AD FS 的內容中，Web 應用程式 Proxy 可作為 AD FS 同盟伺服器 proxy。 此外，Web 應用程式 Proxy 為您公司網路內部的 Web 應用程式提供反向 Proxy 功能，以讓任何裝置上的使用者能從公司網路外部存取這些應用程式。 如需 Web 應用程式 Proxy 的詳細資訊，請參閱 Web 應用程式 Proxy 概觀。

若要規劃 Web 應用程式 Proxy 的部署，您可以檢閱下列主題的資訊：

-   [規劃 Web 應用程式 Proxy 基礎結構 (WAP) ](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))

-   [規劃 Web 應用程式 Proxy 伺服器](/previous-versions/orphan-topics/ws.11/dn383647(v=ws.11))

若要部署 Web Application Proxy，您可以依照下列主題的程序：

-   [設定 Web 應用程式 Proxy 基礎結構](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383644(v=ws.11))

-   [安裝和設定 Web 應用程式 Proxy 伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383662(v=ws.11))


## <a name="see-also"></a>另請參閱

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)

