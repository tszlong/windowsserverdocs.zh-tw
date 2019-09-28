---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: 將您的部署目標對應至 AD FS 設計
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ca10f8e784fea3b99a60b2117f65ba1ccaf6501e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359094"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>將您的部署目標對應至 AD FS 設計


在您完成檢查現有的 Active Directory 同盟服務 @no__t 0AD FS @ no__t-1 部署目標，並判斷哪些目標與您的部署有關，您可以將這些目標對應至特定的 AD FS 設計。 如需 AD FS 預先定義之部署目標的詳細資訊，請參閱[識別您的 AD FS 部署目標](Identifying-Your-AD-FS-Deployment-Goals.md)。  
  
使用下表來判斷哪一個 AD FS 設計對應至組織 AD FS 部署目標的適當組合。 此資料表僅指兩個主要 AD FS 設計，如本指南中所述。 不過，您可以使用任何組合的 AD FS 部署目標來建立混合式或自訂 AD FS 設計，以符合組織的需求。  
  
|AD FS 部署目標|[網頁 SSO 設計](Web-SSO-Design.md)|[同盟網頁 SSO 設計](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[為 Active Directory 使用者提供宣告感知應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|否|是，在帳戶夥伴中|  
|[為 Active Directory 使用者提供其他組織的應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|否|是，在帳戶夥伴選用|  
|[為其他組織的使用者提供您的宣告感知應用程式與服務的存取權](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|是|是|  

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

