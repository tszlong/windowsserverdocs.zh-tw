---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: 在 AD FS 中部署同盟伺服器 proxy
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 3329daca0c8b997d7eb67cb6c85a5a11f3ac96ed
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962902"
---
# <a name="deploying-legacy-ad-fs-federation-server-proxies"></a>部署舊版 AD FS 同盟伺服器 proxy

若要在 Active Directory 同盟服務 AD FS 中部署同盟伺服器 \( \) Proxy，請完成[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)中的每個工作。

> [!NOTE]
> 當您使用此檢查清單時，建議您先閱讀[Windows server 2012 中 AD FS 設計指南中](../design/ad-fs-design-guide-in-windows-server-2012.md)的同盟伺服器 proxy 規劃指引的參考，然後再開始設定伺服器的程式。 遵循檢查清單可進一步瞭解同盟伺服器 proxy 的設計和部署程式。

## <a name="about-federation-server-proxies"></a>關於同盟伺服器 proxy
同盟伺服器 proxy 是執行 Windows Server 2012 的電腦 &reg; ，以及已手動設定以在 proxy 角色中採取行動的 AD FS 軟體。 您可以在組織中使用同盟伺服器 Proxy 來提供網際網路用戶端與公司網路防火牆後方同盟伺服器之間的媒介服務。

> [!NOTE]
> 雖然同盟伺服器和同盟伺服器 proxy 角色無法安裝在同一部電腦上，但是同盟伺服器可以執行同盟伺服器 proxy 功能。 如需詳細資訊，請參閱＜ [When to Create a Federation Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807101(v=ws.11))＞。

在 Windows Server 2012 電腦上安裝 AD FS 軟體並將 &reg; 它設定為以 proxy 角色提供服務的動作，會使該電腦成為同盟伺服器 proxy。

