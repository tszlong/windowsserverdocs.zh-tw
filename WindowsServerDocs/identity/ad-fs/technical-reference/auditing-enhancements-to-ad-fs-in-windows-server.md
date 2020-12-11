---
description: 深入瞭解： Windows Server 2016 中 AD FS 的增強功能
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Windows Server 2016 AD FS 的稽核增強功能
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.openlocfilehash: 1281ce03b291748b093ea491f54e6e7924e03cde
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050396"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Windows Server 2016 AD FS 的稽核增強功能

目前，在 Windows Server 2012 R2 的 AD FS 中，有許多針對單一要求產生的 audit 事件，以及有關登入或權杖發行活動的相關資訊在某些 AD FS) 或散佈于多個審核事件中不存在 (。 AD FS 稽核事件因其資訊龐雜的特性，會預設為關閉。

隨著 Windows Server 2016 中的 AD FS 版本的推出，經過更簡化且較不詳細的審核功能。

## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Windows Server 2016 AD FS 的審核等級
根據預設，Windows Server 2016 中的 AD FS 已啟用基本的審核。  在基本的審核中，系統管理員會看到單一要求的5個或更少事件。  這會造成系統管理員必須查看的事件數目明顯降低，以便查看單一要求。   您可以使用 PowerShell cmdlt 來引發或減少審核層級： Set-AdfsProperties-AuditLevel。  下表說明可用的審核層級。

| 稽核層級 | PowerShell 語法 | 描述 |
|--|--|--|
| 無 | Set-AdfsProperties-AuditLevel None | 停用審核，而且不會記錄任何事件。 |
| 基本 (預設)  | Set-AdfsProperties-AuditLevel Basic | 單一要求不會記錄超過5個事件 |
| 「詳細資訊」 | Set-AdfsProperties-AuditLevel 詳細資訊 | 將會記錄所有事件。  這會為每個要求記錄大量的資訊。 |

若要查看目前的審核層級，您可以使用 PowerShell cmdlt： Set-adfsproperties。

![audit 增強功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)

您可以使用 PowerShell cmdlt 來引發或減少審核層級： Set-AdfsProperties-AuditLevel。

![audit 增強功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)

## <a name="types-of-audit-events"></a>Audit 事件的類型
AD FS Audit 事件可以是不同的類型，根據 AD FS 處理的不同要求類型而定。 每個類型的 Audit 事件都有相關聯的特定資料。  您可以在登入要求之間區別審核事件的類型 (亦即權杖要求) 與系統要求 (伺服器呼叫，包括) 的提取設定資訊。

下表說明基本的 audit 事件種類。

| Audit 事件種類 | 事件識別碼 | 描述 |
|--|--|--|
| 全新認證驗證成功 | 1202 | 同盟服務成功驗證新認證的要求。 這包括 WS-TRUST、WS-同盟、SAML-P (第一個階段來產生 SSO) 和 OAuth 授權端點。 |
| 全新認證驗證錯誤 | 1203 | 同盟服務上的全新認證驗證失敗的要求。 這包括 WS-TRUST、WS-饋送、SAML-P (第一個階段來產生 SSO) 和 OAuth 授權端點。 |
| 應用程式權杖成功 | 1200 | 同盟服務成功發出安全性權杖的要求。 針對 WS-同盟，SAML-P 會在使用 SSO 成品處理要求時記錄。  (，例如 SSO cookie) 。 |
| 應用程式權杖失敗 | 1201 | 同盟服務上安全性權杖發行失敗的要求。 針對 WS-同盟，SAML-P 會在使用 SSO 成品處理要求時記錄。  (，例如 SSO cookie) 。 |
| 密碼變更要求成功 | 1204 | 同盟服務已成功處理密碼變更要求的交易。 |
| 密碼變更要求錯誤 | 1205 | 同盟服務無法處理密碼變更要求的交易。 |
| 登出成功 | 1206 | 描述成功的登出要求。 |
| 登出失敗 | 1207 | 描述失敗的登出要求。 |
