---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: "將您的部署目標對應至 AD FS 設計"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>將您的部署目標對應至 AD FS 設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您完成審查現有的 Active Directory 同盟服務 \(AD FS\) 部署目標，您可以判斷您的部署相關的目標是之後，您可以將這些目標對應到特定的 AD FS 設計。 詳細資訊 AD FS 預先定義的部署目標，請查看[找出您 AD FS 部署目標](Identifying-Your-AD-FS-Deployment-Goals.md)。  
  
使用下表判斷您的組織部署目標 AD FS 適當組合哪些 AD FS 設計地圖。 此表格指的是只兩個主要 AD FS 的設計帶有，這個節目表中所述。 不過，您可以建立混合式或自訂 AD FS 設計使用 AD FS 部署目標的任何組合符合您的組織的需求。  
  
|AD FS 部署目標|[Web SSO 設計](Web-SSO-Design.md)|[聯盟的網路 SSO 設計](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[提供您 Active Directory 的使用者存取您的宣告感知應用程式與服務](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|否]|是的在 account 合作夥伴|  
|[提供您的 Active Directory 使用者存取權的應用程式與其他公司的服務](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|否]|是的在 account 合作夥伴選用|  
|[在另一部組織存取的使用者提供您宣告感知應用程式與服務](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|[是]|[是]|  

## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

