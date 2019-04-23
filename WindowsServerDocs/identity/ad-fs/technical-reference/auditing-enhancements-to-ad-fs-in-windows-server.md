---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Windows Server 2016 AD FS 的稽核增強功能
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3d622686a3cc34316f0cf5187839785195c2f104
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880229"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Windows Server 2016 AD FS 的稽核增強功能

>適用於：Windows Server 2016

目前，在 AD FS 適用於 Windows Server 2012 R2 都是針對單一要求和記錄檔中的相關資訊產生的多個稽核事件，或權杖發行活動是不存在 （在某些版本的 AD FS 中） 或跨多個稽核事件呈現。 預設的 AD FS 稽核事件會關閉由於其詳細資訊的本質。  
    Windows Server 2016 中的 AD fs 版本中，稽核已變得更有效率又相較之下較為。  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>在適用於 Windows Server 2016 的 AD FS 的稽核層級  
根據預設，Windows Server 2016 中的 AD FS 會具有基本的稽核已啟用。  基本稽核時，系統管理員會看到針對單一要求的 5 個以下的事件。  這會將標示大幅降低系統管理員可以查看，若要查看單一要求的事件數目。   稽核層級可能會引發，或降低使用 PowerShell cmdlet:Set-AdfsProperties -AuditLevel.  下表說明可用的稽核層級。  
  
||||  
|-|-|-|  
|稽核層級|PowerShell 語法|描述|  
|None|Set-adfsproperties-AuditLevel None|已停用稽核，而且會記錄任何事件。|  
|基本 （預設值）|Set-AdfsProperties - AuditLevel Basic|單一要求就會記錄 5 個以上的事件|  
|Verbose|Set-adfsproperties-AuditLevel 詳細資訊|會記錄所有事件。  它會記錄大量的每個要求的資訊。|  
  
若要檢視目前的稽核層級，您可以使用 PowerShell cmdlet:Get-AdfsProperties.  
  
![稽核增強功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
稽核層級可能會引發，或降低使用 PowerShell cmdlet:Set-AdfsProperties -AuditLevel.  
  
![稽核增強功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>稽核事件類型  
AD FS 稽核事件可以屬於不同的類型，根據不同的型別，AD FS 所處理的要求。 稽核事件的每個類型都有與其相關聯的特定資料。  稽核事件的型別可以區別登入要求 （也就是權杖要求），與系統要求 （包括提取組態資訊的伺服器呼叫）。    
  下表說明基本的稽核事件類型。  
  
||||  
|-|-|-|  
|稽核事件類型|事件識別碼|描述|  
|全新的認證驗證成功|1202|其中最新的認證會驗證 Federation service 的成功要求。 這包括 Ws-trust、 WS-同盟、 SAML-P （第一個階段，以產生 SSO） 和 OAuth 授權端點。|  
|全新的認證驗證錯誤|1203|要求，Federation Service 全新的認證驗證失敗的位置。 這包括 Ws-trust、 Ws-fed、 SAML-P （第一個階段，以產生 SSO） 和 OAuth 授權端點。|  
|應用程式權杖成功|1200|安全性權杖已成功發出 Federation Service 所要求。 WS-同盟，SAML-P SSO 成品處理要求時，這會記錄。 （例如 SSO cookie)。|  
|應用程式的權杖失敗|1201|要求，Federation Service 的安全性權杖發行失敗的位置。 WS-同盟，SAML-P SSO 成品處理要求時，這會記錄。 （例如 SSO cookie)。|  
|密碼變更要求成功|1204|密碼變更要求的其中一個交易已成功處理同盟服務。|  
|密碼變更要求時發生錯誤|1205|密碼變更要求的其中一個交易無法 Federation Service 所處理。| 
|登出成功|1206|描述成功登出要求。|  
|登出失敗|1207|描述失敗的登出要求。|  

  


