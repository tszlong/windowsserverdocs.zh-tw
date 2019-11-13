---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: 規劃與 AD FS 1.x 的互通性
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e9f72bd83c90a804749329521a72e3232589c735
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407965"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>規劃與 AD FS 1.x 的互通性

Active Directory 同盟服務 \(AD FS\) 執行 Windows Server®2012的同盟伺服器可以與 Windows Server 2003 R2 AD FS \(安裝的\) 1.0 同盟服務和 windows server 2008 或 Windows Server 2008 R2 AD FS \(一起安裝的\) 1.1 同盟服務互通。 支援下列任何互通性組合：  

-   任何 AD FS 1。*x*同盟服務可以傳送可由 Windows Server 2012 中的 AD FS 同盟服務使用的宣告。 如需詳細資訊，請參閱[檢查清單：設定 AD FS 以取用來自 AD FS](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)1.X 的宣告。  

-   Windows Server 2012 中的任何 AD FS 同盟服務都可以傳送 AD FS 1。*x*\-相容的宣告，可由 AD FS 1 使用。*x*同盟服務。 如需詳細資訊，請參閱 [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Federation Service](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。  

-   Windows Server 2012 中的任何 AD FS 同盟服務都可以傳送 AD FS 1。*x*\-相容的宣告，可供一或多部執行 AD FS 1 的 Web 服務器使用。*x*宣告\-感知 Web 代理程式。 如需詳細資訊，請參閱 [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Claims-Aware Web Agent](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。  

> [!NOTE]  
> AD FS 不支援或與 AD FS 1 相交互操作。*x* Windows NT 權杖型 Web 代理程式。  

AD FS 1。*x*\-相容宣告是一種宣告，可由 Windows Server 2012 中的 AD FS 同盟服務傳送，並由 AD FS 1 瞭解。*x*同盟服務。 AD FS 1。*x*同盟服務可以使用 AD FS 同盟服務所傳送的宣告，必須將名稱識別碼 \(ID\) 宣告類型。  

## <a name="understanding-the-name-id-claim-type"></a>了解名稱識別碼宣告類型  
名稱識別碼宣告類型相當於 AD FS 1.*x* 使用的身分識別宣告類型。 每當您想要與 AD FS 1.*x*交互操作時必須使用它。 名稱識別碼宣告類型會啟用 AD FS 1。*x*同盟服務或 AD FS 1。*x*宣告\-感知 Web 代理程式取用在 Windows Server 2012 中 AD FS 的宣告，只要這些宣告是以下表中的其中一個名稱識別碼格式傳送。  


|      名稱識別碼格式       |               對應的 URI                |
|---------------------------|------------------------------------------------|
| AD FS 1.*x* 電子郵件地址 | http://schemas.xmlsoap.org/claims/EmailAddress |
|   AD FS 1.*x* 電子郵件 UPN   |     http://schemas.xmlsoap.org/claims/UPN      |
|        一般名稱        |  http://schemas.xmlsoap.org/claims/CommonName  |
|           群組           |    http://schemas.xmlsoap.org/claims/Group     |

僅必須以適當的格式傳送名稱識別碼宣告。 滿足該準則時，可能也會傳送許多其他的宣告，假設它們符合資料表中所述的限制。  

> [!NOTE]  
> AD FS 1。*x*同盟服務只能解讀以統一資源識別元開頭的傳入宣告類型 \(URI\) 的 http://schemas.xmlsoap.org/claims/。  

## <a name="see-also"></a>請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
