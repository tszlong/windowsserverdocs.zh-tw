---
description: 深入瞭解：將您的部署目標對應至 AD FS 設計
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: 將您的部署目標對應至 AD FS 設計
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 6bc9e22075c618871d3a0fdd43c6677ddcc62b83
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041376"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>將您的部署目標對應至 AD FS 設計


當您完成審核現有的 Active Directory 同盟服務 \( AD FS \) 部署目標，並判斷哪些目標與您的部署有關時，可以將這些目標對應至特定的 AD FS 設計。 如需 AD FS 預先定義的部署目標的詳細資訊，請參閱 [識別 AD FS 部署目標](Identifying-Your-AD-FS-Deployment-Goals.md)。

您可以使用下表來判斷哪一個 AD FS design 對應到您組織 AD FS 部署目標的適當組合。 此資料表只會參考這兩個主要 AD FS 設計，如本指南所述。 不過，您可以使用 AD FS 部署目標的任意組合來建立混合式或自訂 AD FS 設計，以符合組織的需求。

|AD FS 部署目標|[網頁 SSO 設計](Web-SSO-Design.md)|[同盟網頁 SSO 設計](Federated-Web-SSO-Design.md)|
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
|[為 Active Directory 使用者提供宣告感知應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|否|是，在帳戶夥伴中|
|[為 Active Directory 使用者提供其他組織的應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|否|是，在帳戶夥伴選用|
|[為其他組織的使用者提供您的宣告感知應用程式與服務的存取權](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|是|是|

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)


