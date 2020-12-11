---
description: 深入瞭解：檢查資源夥伴中的同盟伺服器 Proxy 角色
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: 檢閱資源夥伴中的同盟伺服器 Proxy 角色
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d0419028c4b2a9cc411cef81dabbb0ddb2da7044
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049966"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>檢閱資源夥伴中的同盟伺服器 Proxy 角色

Active Directory 同盟服務 AD FS 中的同盟伺服器 \( proxy \) 可以在下列一或多個角色中運作，視您如何設定伺服器以符合資源夥伴組織的需求而定：

-   **帳戶夥伴探索**：網際網路用戶端電腦必須識別哪一個帳戶夥伴將會驗證它。 用戶端會使用帳戶夥伴探索 Web 表單 \( discoverclientrealm \) （儲存在資源夥伴的同盟伺服器 proxy 上）來尋找帳戶夥伴。 如果在 AD FS 管理] 嵌入式管理單元中設定一個以上的帳戶夥伴 \- ， \- 則會向用戶端顯示一個下拉式功能表，其中包含可存取帳戶夥伴探索 Web 表單的網際網路用戶端電腦看到的所有可用帳戶夥伴。 您可以自訂 discoverclientrealm.aspx 檔案，以變更帳戶夥伴探索 Web 表單對用戶端電腦的呈現方式。

-   **安全性權杖** 重新導向：帳戶夥伴中的同盟伺服器 proxy 會將安全性權杖傳送給資源夥伴。 資源同盟伺服器 proxy 接受這些權杖，並將它們傳遞給資源夥伴中的同盟伺服器。 然後，資源同盟伺服器會發出系結至特定資源 Web 服務器的安全性權杖。 然後，資源同盟伺服器 proxy 會將權杖重新導向至用戶端。

總而言之，資源同盟伺服器 proxy 會將用戶端電腦重新導向至可驗證用戶端的同盟伺服器，以協助同盟登入程式。 資源同盟伺服器 proxy 也可作為資源同盟伺服器之用戶端安全性權杖的 proxy。

> [!NOTE]
> 當您需要協助減少硬體數量和所需的憑證數目時，同盟伺服器 proxy 可以與 Web 服務器位於同一部電腦上。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

