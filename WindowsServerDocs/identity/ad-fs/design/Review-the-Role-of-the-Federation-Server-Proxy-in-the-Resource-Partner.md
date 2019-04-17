---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: "檢視聯盟伺服器 Proxy 資源夥伴中的角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 31e285e863e4316a8e0a65f9b68c27442290927d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>檢視聯盟伺服器 Proxy 資源夥伴中的角色

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory 同盟服務 \(AD FS\) 聯盟伺服器 proxy 可以一或多個下列的角色，根據您如何設定需求資源合作夥伴組織的伺服器功能：  
  
-   **Account 合作夥伴探索**: client 的網際網路的電腦必須找出您所 account 合作夥伴將驗證它。 Client 使用 account 合作夥伴探索 Web 表單 \(discoverclientrealm.aspx\)、儲存在聯盟伺服器 proxy 資源夥伴中尋找 account 合作夥伴。 如果超過一個 account 合作夥伴 snap\ 中，向下 drop\ 功能表似乎 client 與看到網際網路存取 account 合作夥伴探索 Web 表單 client 電腦的所有可用 account 協力廠商 AD FS 管理設定。 您可以變更 account 合作夥伴探索 Web 表單顯示的方式 client 電腦自訂 discoverclientrealm.aspx 檔案。  
  
-   **安全性權杖重新導向**：聯盟伺服器 proxy account 合作夥伴中的將的安全性權杖傳送到資源合作夥伴。 資源聯盟伺服器 proxy 接受這些權杖和傳遞到資源合作夥伴聯盟伺服器。 資源聯盟伺服器然後問題的安全性權杖繫結的特定資源網頁伺服器。 資源聯盟伺服器 proxy 再重新導向至 amc 權杖 client。  
  
總結資源聯盟 proxy 伺服器幫助您藉由驗證戶端聯盟伺服器重新導向 client 電腦的登入聯盟程序。 資源聯盟 proxy 伺服器也做為 proxy client 的安全性權杖給資源聯盟伺服器。  
  
> [!NOTE]  
> 以協助降低的硬體和數目需要憑證必要時，可以位於聯盟 proxy 伺服器相同的電腦與 Web 伺服器上。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

