---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: "AD FS 使用的跨平台規劃 1.x"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f287261ce6cb56e40385ef4de922045153819a23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>AD FS 使用的跨平台規劃 1.x

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory 同盟服務 \(AD FS\) 聯盟伺服器執行 Windows Server® 2012 年能與這兩個 AD FS 1.0 交互操作 \（與 Windows Server 2003 R2\ 安裝）同盟服務與 AD FS 1.1 \（與 Windows Server 2008 或 Windows Server 2008 R2\ 安裝）同盟服務。 支援下列組合交互操作：  
  
-   任何 AD FS 1。*x*同盟服務可以傳送，可以使用 Windows Server 2012 中 AD FS 同盟服務理賠要求。 如需詳細資訊，請查看[檢查清單︰ 設定從 AD FS AD FS 使用宣告 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)。  
  
-   Windows Server 2012 中的所有 AD FS 同盟服務可以傳送都給 AD FS 1。*x*，可由 AD FS 1 \-compatible 理賠要求。*x*同盟服務。 如需詳細資訊，請查看[檢查清單︰ 設定來傳送給 AD FS 以的宣告 AD FS 1.x 同盟服務](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。  
  
-   Windows Server 2012 中的所有 AD FS 同盟服務可以傳送都給 AD FS 1。*x*，可由執行 AD FS 1 一或多個 Web 伺服器 \-compatible 理賠要求。*x* claims\ 感知 Web 代理程式。 如需詳細資訊，請查看[檢查清單︰ 設定來傳送給 AD FS 以的宣告 AD FS 1.x 宣告感知 Web 代理程式](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。  
  
> [!NOTE]  
> AD FS 不支援或交互 1 AD FS 進行操作。*x* Windows NT 權杖型 Web 代理程式。  
  
AD FS 1。*x*\-compatible 宣告，可以在 Windows Server 2012 中 AD FS 同盟服務，傳送並了解 AD FS 1 理賠要求。*x*同盟服務。 因此，AD FS 1。*x*同盟服務可以使用 AD FS 同盟服務會將傳送宣告，名稱 Identifier \(ID\) 宣告類型必須傳送。  
  
## <a name="understanding-the-name-id-claim-type"></a>了解名稱 ID 宣告類型  
名稱 ID 宣告類型是相當於的身分宣告輸入該 AD FS 1。*x* uses. 每當您想要使用 AD FS 1 交互必須使用它。*x*. 名稱 ID 理賠要求輸入可讓 AD FS 1。*x*同盟服務或 AD FS 1。*x*以取用宣告傳送給 AD FS Windows Server 2012 中的，只要這些宣告傳送下表中名稱 ID 格式之一 claims\ 感知 Web 代理程式。  
  
|來電顯示名稱的格式|對應 URI|  
|------------------|---------------------|  
|AD FS 1。*x*電子郵件地址|http://schemas.xmlsoap.org/claims/EmailAddress|  
|AD FS 1。*x* UPN 電子郵件|http://schemas.xmlsoap.org/claims/UPN|  
|一般的名稱|http://schemas.xmlsoap.org/claims/CommonName|  
|群組|http://schemas.xmlsoap.org/claims/Group|  
  
在適當的格式只有一個名稱 ID 宣告必須傳送。 當符合的條件是時，許多其他宣告可能會傳送，假設它們符合表格中所述的限制。  
  
> [!NOTE]  
> AD FS 1。*x*同盟服務可以解譯開頭的 http://schemas.xmlsoap.org/claims/ 統一資源識別碼 \(URI\) 只傳入宣告類型。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
