---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: 檢閱資源夥伴中的同盟伺服器 Proxy 角色
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6f49677df16cb1b44d08449a3983ae29fed3cb27
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858541"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>檢閱資源夥伴中的同盟伺服器 Proxy 角色

Active Directory 同盟服務 \(AD FS\) 中的同盟伺服器 proxy 可在下列一個或多個角色中運作，視您如何設定伺服器以符合資源夥伴組織的需求而定：  
  
-   **帳戶夥伴探索**：網際網路用戶端電腦必須識別哪一個帳戶夥伴將會驗證它。 用戶端會使用帳戶夥伴探索 Web 表單 \(\)discoverclientrealm.aspx （儲存在資源夥伴的同盟伺服器 proxy 上）來尋找帳戶夥伴。 如果在的 [AD FS 管理] 嵌入式\-管理單元中設定了一個以上的帳戶夥伴，則會向用戶端顯示一個\-下拉式功能表，其中包含存取帳戶夥伴探索 Web 表單的網際網路用戶端電腦所能看見的所有可用帳戶夥伴。 您可以自訂 discoverclientrealm.aspx 檔案，以變更帳戶夥伴探索 Web 表單對用戶端電腦的呈現方式。  
  
-   **安全性權杖**重新導向：帳戶夥伴中的同盟伺服器 proxy 會將安全性權杖傳送給資源夥伴。 資源同盟伺服器 proxy 會接受這些權杖，並將它們傳遞到資源夥伴中的同盟伺服器。 然後，資源同盟伺服器會發出系結至特定資源網頁伺服器的安全性權杖。 然後，資源同盟伺服器 proxy 會將權杖重新導向至用戶端。  
  
總結而言，資源同盟伺服器 proxy 藉由將用戶端電腦重新導向至可驗證用戶端的同盟伺服器，來協助聯盟登入程式。 資源同盟伺服器 proxy 也會做為資源同盟伺服器之用戶端安全性權杖的 proxy。  
  
> [!NOTE]  
> 當您需要協助減少硬體數量和所需的憑證數目時，同盟伺服器 proxy 可以與網頁伺服器位於同一部電腦上。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

