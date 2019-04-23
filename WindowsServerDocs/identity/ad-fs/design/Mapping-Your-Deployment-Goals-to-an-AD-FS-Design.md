---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: 將您的部署目標對應至 AD FS 設計
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866819"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>將您的部署目標對應至 AD FS 設計

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在您完成檢閱現有的 Active Directory Federation Services 後\(AD FS\)部署目標且判斷哪些目標會與您的部署，您可以將這些目標對應至特定的 AD FS 設計。 如需 AD FS 的詳細資訊，預先定義部署目標，請參閱 <<c0> [ 找出您的 AD FS 部署目標](Identifying-Your-AD-FS-Deployment-Goals.md)。  
  
使用下表來判斷哪一個 AD FS 設計會對應至適當的 AD FS 的組合為您的組織部署目標。 此資料表只適用於兩個主要 AD FS 設計中，本指南中所述。 不過，您可以建立混合式或自訂的 AD FS 設計使用 AD FS 部署目標的任何組合，以符合您組織的需求。  
  
|AD FS 部署目標|[網頁 SSO 設計](Web-SSO-Design.md)|[同盟的網頁 SSO 設計](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[提供您的 Active Directory 使用者存取您的宣告感知應用程式和服務](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|否|是，在帳戶夥伴中|  
|[您 Active Directory 使用者提供存取權的應用程式和其他組織的服務](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|否|是，在帳戶夥伴選用|  
|[提供使用者在另一個組織存取您宣告感知應用程式和服務](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|是|是|  

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

