---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: 規劃與 AD FS 1.x 的互通性
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7a1082b873f65a9f98b25425a392b2c62de8ca22
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191011"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>規劃與 AD FS 1.x 的互通性

Active Directory Federation Services \(AD FS\)執行 Windows Server® 2012年的同盟伺服器可以與這兩個 AD FS 1.0 交互操作\(隨 Windows Server 2003 R2\) Federation Service 與 AD FS1.1\(與 Windows Server 2008 或 Windows Server 2008 R2 一起安裝\)Federation Service。 支援下列任何互通性組合：  
  
-   任何 AD FS 1。*x*同盟服務可傳送可由 Windows Server 2012 中 AD FS Federation Service 的宣告。 如需詳細資訊，請參閱[檢查清單：設定 AD FS 使用宣告從 AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)。  
  
-   Windows Server 2012 中的任何 AD FS 同盟服務可以傳送 AD FS 1。*x*\-相容的宣告可以由 AD FS 1。*x* Federation Service。 如需詳細資訊，請參閱[檢查清單：設定 AD FS 來傳送至 AD FS 1.x Federation Service 的宣告](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。  
  
-   Windows Server 2012 中的任何 AD FS 同盟服務可以傳送 AD FS 1。*x*\-相容的宣告，可供一個或多個 Web 伺服器執行 AD FS 1。*x*宣告\-感知網路代理程式。 如需詳細資訊，請參閱[檢查清單：設定 AD FS 以將宣告傳送至 AD FS 1.x Claims-aware Web 代理程式](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。  
  
> [!NOTE]  
> AD FS 不支援或與 AD FS 1 交互操作。*x* Windows NT 權杖型網路代理程式。  
  
AD FS 1。*x*\-相容的宣告是可以了解 AD fs 1 和 Windows Server 2012 中 AD FS 同盟服務所傳送的宣告。*x* Federation Service。 因此，AD FS 1。*x* Federation Service 可以使用的宣告，AD FS 同盟服務傳送，名稱識別項\(識別碼\)宣告必須傳送的類型。  
  
## <a name="understanding-the-nameid-claim-type"></a>了解名稱識別碼宣告類型  
名稱識別碼宣告類型相當於 AD FS 1.*x* 使用的身分識別宣告類型。 每當您想要與 AD FS 1.*x* 交互操作時必須使用它。 名稱識別碼宣告類型是可讓 AD FS 1。*x*同盟服務或 AD FS 1。*x*宣告\-，只要這些宣告會以其中一個下表中的名稱識別碼格式傳送，請使用 Windows Server 2012 中的 AD FS 傳送的宣告感知網路代理程式。  
  
|名稱識別碼格式|對應的 URI|  
|------------------|---------------------|  
|AD FS 1.*x* 電子郵件地址|http://schemas.xmlsoap.org/claims/EmailAddress|  
|AD FS 1.*x* 電子郵件 UPN|http://schemas.xmlsoap.org/claims/UPN|  
|一般名稱|http://schemas.xmlsoap.org/claims/CommonName|  
|群組|http://schemas.xmlsoap.org/claims/Group|  
  
僅必須以適當的格式傳送名稱識別碼宣告。 滿足該準則時，可能也會傳送許多其他的宣告，假設它們符合資料表中所述的限制。  
  
> [!NOTE]  
> AD FS 1。*x*同盟服務可解譯開頭為統一資源識別項的僅傳入宣告類型\(URI\)的 http://schemas.xmlsoap.org/claims/。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
