---
description: 深入瞭解：部署舊版 AD FS 同盟伺服器 proxy
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: 在 AD FS 中部署同盟伺服器 proxy
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 7798867f8445ee99835e883ccab771fa346b2673
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044126"
---
# <a name="deploying-legacy-ad-fs-federation-server-proxies"></a>部署舊版 AD FS 同盟伺服器 proxy

若要在 Active Directory 同盟服務 AD FS 中部署同盟伺服器 \( \) Proxy，請完成檢查清單中的每個工作 [：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)。

> [!NOTE]
> 當您使用這份檢查清單時，建議您先閱讀 [Windows server 2012 的《 AD FS 設計指南》](../design/ad-fs-design-guide-in-windows-server-2012.md) 中的同盟伺服器 proxy 規劃指南的參考，然後再開始設定伺服器的程式。 遵循檢查清單可進一步瞭解同盟伺服器 proxy 的設計和部署程式。

## <a name="about-federation-server-proxies"></a>關於同盟伺服器 proxy
同盟伺服器 proxy 是執行 Windows Server 2012 的電腦， &reg; 以及已手動設定為在 proxy 角色中採取行動的 AD FS 軟體。 您可以在組織中使用同盟伺服器 Proxy 來提供網際網路用戶端與公司網路防火牆後方同盟伺服器之間的媒介服務。

> [!NOTE]
> 雖然同盟伺服器和同盟伺服器 proxy 角色無法安裝在同一部電腦上，但同盟伺服器可以執行同盟伺服器 proxy 功能。 如需詳細資訊，請參閱＜ [When to Create a Federation Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807101(v=ws.11))＞。

將 AD FS 軟體安裝在 Windows Server &reg; 2012 電腦上，並將它設定為在 proxy 角色中提供的動作，會讓該電腦成為同盟伺服器 proxy。

