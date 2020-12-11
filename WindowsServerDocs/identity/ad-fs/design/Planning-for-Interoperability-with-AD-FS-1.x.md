---
description: 深入瞭解：規劃 AD FS 1.x 的互通性
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: 規劃與 AD FS 1.x 的互通性
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e956d66dd528ed73e4adcdfb8d9d04f4cab1666d
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049696"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>規劃與 AD FS 1.x 的互通性

\( \) 執行 windows server 2012 Active Directory 同盟服務 AD FS 同盟伺服器 &reg; 可以與 \( windows server 2003 R2 同盟服務所安裝的 AD FS 1.0 \) 以及 \( windows Server 2008 或 windows server 2008 R2 AD FS 一起安裝的同盟服務1.1 進行交互操作 \) 。 支援下列任何互通性組合：

-   任何 AD FS 1。*x* 同盟服務可以傳送可供 Windows Server 2012 中的 AD FS 同盟服務使用的宣告。 如需詳細資訊，請參閱 [檢查清單：設定 AD FS 從 AD FS 1.X 使用宣告](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)。

-   Windows Server 2012 中的任何 AD FS 同盟服務都可以傳送 AD FS 1。*x* \-AD FS 1 可使用的相容宣告。*x* 同盟服務。 如需詳細資訊，請參閱 [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Federation Service](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。

-   Windows Server 2012 中的任何 AD FS 同盟服務都可以傳送 AD FS 1。*x* \-可供一或多部執行 AD FS 1 的 Web 服務器使用的相容宣告。*x* 宣告 \- 感知 Web 代理程式。 如需詳細資訊，請參閱 [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Claims-Aware Web Agent](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。

> [!NOTE]
> AD FS 不支援或交互操作 AD FS 1。以權杖為基礎的 Web 代理程式 Windows NT *x* 。

AD FS 1。*x* \-相容宣告是可由 Windows Server 2012 中的 AD FS 同盟服務所傳送並由 AD FS 1 所瞭解的宣告。*x* 同盟服務。 AD FS 1。*x* 同盟服務可以使用 AD FS 同盟服務傳送的宣告，必須傳送名稱識別碼識別碼 \( 宣告 \) 類型。

## <a name="understanding-the-name-id-claim-type"></a>了解名稱識別碼宣告類型
名稱識別碼宣告類型相當於 AD FS 1.*x* 使用的身分識別宣告類型。 每當您想要與 AD FS 1.*x* 交互操作時必須使用它。 名稱識別碼宣告類型會啟用 AD FS 1。*x* 同盟服務或 AD FS 1。*x* 宣告 \- 感知 Web 代理程式，用來取用 Windows Server 2012 中 AD FS 的宣告，只要以下表中的其中一個名稱識別碼格式來傳送這些宣告即可。


|      名稱識別碼格式       |               對應的 URI                |
|---------------------------|------------------------------------------------|
| AD FS 1.*x* 電子郵件地址 | http://schemas.xmlsoap.org/claims/EmailAddress |
|   AD FS 1.*x* 電子郵件 UPN   |     http://schemas.xmlsoap.org/claims/UPN      |
|        一般名稱        |  http://schemas.xmlsoap.org/claims/CommonName  |
|           群組           |    http://schemas.xmlsoap.org/claims/Group     |

僅必須以適當的格式傳送名稱識別碼宣告。 滿足該準則時，可能也會傳送許多其他的宣告，假設它們符合資料表中所述的限制。

> [!NOTE]
> AD FS 1。*x* 同盟服務只能解讀以的統一資源識別項 URI 開頭的傳入宣告類型 \( \) http://schemas.xmlsoap.org/claims/ 。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
