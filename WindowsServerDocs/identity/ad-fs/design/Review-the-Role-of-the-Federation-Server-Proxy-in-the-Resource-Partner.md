---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: 檢閱資源夥伴中的同盟伺服器 Proxy 角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 377baa8f282f3886284a53b686944fe145b1b15e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190894"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>檢閱資源夥伴中的同盟伺服器 Proxy 角色

在 Active Directory Federation Services 中的同盟伺服器 proxy \(AD FS\)可以函式中一或多個下列角色，根據您設定伺服器以符合資源夥伴組織的需求的方式：  
  
-   **帳戶夥伴探索**：網際網路用戶端電腦必須識別哪一個帳戶夥伴將會驗證它。 用戶端找到帳戶夥伴所使用帳戶夥伴探索 Web 表單\(discoverclientrealm.aspx\)，其會儲存在資源夥伴同盟伺服器 proxy 上。 如果設定多個帳戶夥伴中 AD FS 管理嵌入式管理單元\-中，卸除\-下功能表會顯示有權存取帳戶夥伴的網際網路用戶端電腦的所有可用帳戶夥伴的用戶端探索 Web 表單。 您可以自訂 discoverclientrealm.aspx 檔案，以變更帳戶夥伴探索 Web 表單對用戶端電腦的呈現方式。  
  
-   **安全性權杖重新導向**：帳戶夥伴中的同盟伺服器 proxy 會將安全性權杖傳送給資源夥伴。 資源同盟伺服器 proxy 會接受這些權杖，並將其傳遞至資源夥伴中的同盟伺服器。 資源同盟伺服器接著會發出特定資源的 Web 伺服器繫結的安全性權杖。 資源同盟伺服器 proxy 接著會將權杖給用戶端重新導向。  
  
若要總而言之，資源同盟伺服器 proxy 會執行同盟登入程序，方法是重新導向至同盟伺服器可驗證用戶端的用戶端電腦。 資源同盟伺服器 proxy 也會作為用戶端安全性權杖的資源同盟伺服器 proxy。  
  
> [!NOTE]  
> 需要協助降低硬體數量和必要的憑證數目時，同盟伺服器 proxy 可以位於與 Web 伺服器相同的電腦。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

