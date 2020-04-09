---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Windows Server 2016 AD FS 的稽核增強功能
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 191ecf5b3c7bf6c8c44d4d3553cd6e98b5543351
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853801"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Windows Server 2016 AD FS 的稽核增強功能


目前，在 Windows Server 2012 R2 的 AD FS 中，有許多針對單一要求產生的 audit 事件，而且登入或權杖發行活動的相關資訊可能不存在（在某些版本的 AD FS 中），或散佈在多個 audit 事件中。 AD FS 稽核事件因其資訊龐雜的特性，會預設為關閉。  
    隨著 Windows Server 2016 中的 AD FS 版本，經過簡化的程式也變得更有效率，而且更不詳細。  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Windows Server 2016 AD FS 中的審核層級  
根據預設，Windows Server 2016 中的 AD FS 已啟用基本的審核。  在基本的審核中，系統管理員會針對單一要求看到5個或更少的事件。  這會導致系統管理員必須查看的事件數目大幅降低，以便看到單一要求。   您可以使用 PowerShell cmdlt 來提高或降低審核層級： Set-adfsproperties-AuditLevel。  下表說明可用的審核層級。  
  
||||  
|-|-|-|  
|稽核層級|PowerShell 語法|描述|  
|無|Set-adfsproperties-AuditLevel None|已停用審核，而且不會記錄任何事件。|  
|基本（預設值）|Set-adfsproperties-AuditLevel Basic|單一要求不會記錄超過5個事件|  
|Verbose|Set-adfsproperties-AuditLevel Verbose|將記錄所有事件。  這會針對每個要求記錄大量的資訊。|  
  
若要查看目前的審核層級，您可以使用 PowerShell cmdlt： Get-Set-adfsproperties。  
  
![audit 增強功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
您可以使用 PowerShell cmdlt 來提高或降低審核層級： Set-adfsproperties-AuditLevel。  
  
![audit 增強功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>Audit 事件的類型  
根據 AD FS 所處理的不同要求類型，AD FS Audit 事件可以是不同類型。 每種類型的 Audit 事件都有相關聯的特定資料。  您可以在登入要求（也就是權杖要求）與系統要求（包括提取設定資訊的伺服器-伺服器呼叫）之間區分 audit 事件的類型。    
  下表描述的是 audit 事件的基本類型。  
  
||||  
|-|-|-|  
|Audit 事件種類|事件識別碼|描述|  
|全新認證驗證成功|1202|同盟服務成功驗證新認證的要求。 這包括 WS-TRUST、WS-同盟、SAML-P （產生 SSO 的第一個階段）和 OAuth 授權端點。|  
|新的認證驗證錯誤|1203|同盟服務上的新認證驗證失敗的要求。 這包括 WS-TRUST、WS-送出、SAML-P （產生 SSO 的第一個階段）和 OAuth 授權端點。|  
|應用程式權杖成功|1200|同盟服務成功發出安全性權杖的要求。 針對 WS-同盟，當使用 SSO 成品處理要求時，會記錄此專案。 （例如 SSO cookie）。|  
|應用程式權杖失敗|1201|同盟服務上安全性權杖發行失敗的要求。 針對 WS-同盟，在使用 SSO 成品處理要求時，會記錄此 SAML-P。 （例如 SSO cookie）。|  
|密碼變更要求成功|1204|同盟服務已成功處理密碼變更要求的交易。|  
|密碼變更要求錯誤|1205|同盟服務無法處理密碼變更要求的交易。| 
|登出成功|1206|說明成功的登出要求。|  
|登出失敗|1207|描述失敗的登出要求。|  

  


