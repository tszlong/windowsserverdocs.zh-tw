---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: "若要在 Windows Server 2016 AD FS 稽核美化效果"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3d622686a3cc34316f0cf5187839785195c2f104
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>若要在 Windows Server 2016 AD FS 稽核美化效果

>適用於：Windows Server 2016

目前，在 AD FS 適用於 Windows Server 2012 R2 的許多稽核事件單一要求和的相關資訊的登入或權杖發行活動是缺少 （在某些 AD FS 版本） 或讓跨多個稽核事件。 AD FS 預設稽核事件是因為其詳細資訊的性質被關閉。  
    在 Windows Server 2016 AD FS 發行，稽核已變更有效率且較低的詳細資訊。  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>稽核 AD FS 適用於 Windows Server 2016 中的層級  
根據預設，Windows Server 2016 中的 AD FS 有基本稽核功能。  基本稽核時，系統管理員將會看到的單一要求 5 或較少的活動。  這表示減少重要的系統管理員可以查看，以查看單一要求的事件數目。   稽核等級可提高或降低使用 PowerShell cmdlt: AdfsProperties 設定-AuditLevel。  下表解釋可稽核的層級。  
  
||||  
|-|-|-|  
|稽核層級|PowerShell 語法|描述|  
|無|設定 AdfsProperties-AuditLevel 無|稽核已停用，並將會登入不事件。|  
|基本 （預設值）|設定 AdfsProperties-AuditLevel Basic|不會超過 5 事件將會登入的單一要求|  
|詳細資訊|設定 AdfsProperties-AuditLevel 詳細資訊|事件所有將會登入。  這將會登入大量的每個要求的資訊。|  
  
若要檢視目前稽核等級，您可以使用 PowerShell cmdlt： 取得-AdfsProperties。  
  
![稽核美化效果](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
稽核等級可提高或降低使用 PowerShell cmdlt: AdfsProperties 設定-AuditLevel。  
  
![稽核美化效果](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>稽核事件類型  
AD FS 稽核事件可以種不同類型，根據不同類型的處理 AD FS 的要求。 稽核事件每一種有特定與其相關聯的資料。  稽核事件類型與系統要求 （包括擷取組態資訊伺服器通話） 可區分之間登入要求 （亦即權杖要求）。    
  下表描述基本稽核活動的類型。  
  
||||  
|-|-|-|  
|稽核事件類型|事件編號|描述|  
|全新 Credential 驗證成功|1202|要求位置新的驗證憑證成功同盟服務。 這包括 Ws-trust，WS 聯盟 SAML-P （產生 SSO 第一側邊） 和 OAuth 授權端點。|  
|全新的認證驗證錯誤|1203|要求同盟服務全新 credential 驗證失敗的位置。 這包括 Ws-trust，WS-Fed、 SAML-P （產生 SSO 第一側邊） 和 OAuth 授權端點。|  
|應用程式權杖成功|1200|要求的安全性權杖成功發出同盟服務。 適用於 WS-同盟，這登入，要求處理 SSO 成品使用時 SAML P。 （例如 SSO cookie)。|  
|應用程式權杖失敗|1201|要求同盟服務的安全性權杖發行失敗位置。 適用於 WS-同盟，這登入時使用的 SSO 成品處理要求 SAML P。 （例如 SSO cookie)。|  
|密碼變更要求成功|1204|變更密碼要求交易已成功處理同盟服務。|  
|密碼變更要求錯誤|1205|變更密碼要求交易無法處理同盟服務。| 
|查看成功登入|1206|告訴您成功 sign-out 要求。|  
|登出失敗|1207|請描述 sign-out 要求失敗。|  

  


