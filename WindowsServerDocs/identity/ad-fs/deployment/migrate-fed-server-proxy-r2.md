---
title: 遷移 AD FS 2.0 同盟 proxy 伺服器
description: 提供將 AD FS proxy 伺服器遷移至 Windows Server 2012 R2 的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.openlocfilehash: a8345f587e7fe4119e46dc390e9240dee6071ce6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940831"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>將 Active Directory 同盟服務 Proxy 伺服器遷移至 Windows Server 2012 R2

在 Windows Server 2012 R2 中的 Active Directory 同盟服務 (AD FS) 中，同盟伺服器 proxy 的角色是由新的遠端存取角色服務（稱為 Web 應用程式 Proxy）所處理。 在 Windows Server 2012 R2 中，若要讓您的 AD FS 可從公司網路外部存取，您可以部署一或多個 Web 應用程式 proxy。 不過，您無法將在 Windows Server 2008 R2 或 Windows Server 2012 上執行的同盟伺服器 proxy，遷移到在 Windows Server 2012 R2 上執行的 Web 應用程式 Proxy。

> [!IMPORTANT]
>  不支援將在 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 上執行的同盟伺服器 proxy 遷移到在 Windows Server 2012 R2 上執行的 Web 應用程式 Proxy。

如果您想要在 Windows Server 2012 R2 遷移的伺服器陣列中設定 AD FS 以進行外部網路存取，您必須在 AD FS 基礎結構中執行一或多個 Web 應用程式 Proxy 電腦的全新部署。

若要規劃 Web Application Proxy部署，您可以檢閱下列主題的資訊：

- [規劃 Web 應用程式 Proxy 基礎結構](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))

- [規劃 Web 應用程式 Proxy 伺服器](/previous-versions/orphan-topics/ws.11/dn383647(v=ws.11))

  若要部署 Web Application Proxy，您可以依照下列主題的程序：

- [設定 Web 應用程式 Proxy 基礎結構](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383644(v=ws.11))

- [安裝和設定 Web 應用程式 Proxy 伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383662(v=ws.11))

## <a name="next-steps"></a>後續步驟
 [將 Active Directory 同盟服務角色服務遷移至 Windows server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md) [準備遷移 AD FS 同盟伺服器](prepare-migrate-ad-fs-server-r2.md)[遷移 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md) [，以確認 AD FS 遷移至 windows server 2012 R2](verify-ad-fs-migration.md)
