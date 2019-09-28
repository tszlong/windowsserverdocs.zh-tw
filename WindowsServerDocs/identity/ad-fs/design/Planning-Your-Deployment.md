---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: 規劃您的部署
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 607dc34c8f44d8d96a8dc0c9d1ed004edc799167
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407999"
---
# <a name="planning-your-deployment"></a>規劃您的部署

當您規劃 cross @ no__t-0organizational \(federation @ no__t-2based @ no__t-3 共同作業使用 Active Directory 同盟服務 \(AD FS @ no__t-5 時，請先判斷您的組織是否會裝載要由其他使用者存取的 Web 資源網際網路上的組織，或如果您將可存取組織中員工的 Web 資源。 這項決定會影響您部署 AD FS 的方式，而且它是規劃 AD FS 基礎結構的基礎。  
  
> [!NOTE]  
> 請確定所有合作對象清楚瞭解組織在同盟協議中所扮演的角色。  
  
在同盟[網頁 SSO 設計](Federated-Web-SSO-Design.md)中，AD FS 會在 AD FS 管理 snap @ no__t-4 英吋 @ no__t-5 和*資源夥伴*\(also *中，使用帳戶夥伴 @no__t 2also，稱為「識別提供者」。* AD FS Management snap @ no__t-9in @ no__t-10 中的信賴憑證者，可協助區分從裝載 Web @ no__t-no__t 資源 @no__t 13based 資源的組織，將裝載帳戶的組織 @no__t 11 帳戶夥伴 @ 14the-12partner @ no__t-15。  
  
在 [Web SSO Design](Web-SSO-Design.md)中，組織會同時作為帳戶夥伴和資源夥伴角色，因為它提供其使用者其應用程式的存取權。  
  
下列主題說明 AD FS 合作夥伴組織的一些概念。 其中也包含 AD FS 部署指南中的主題連結，其中包含根據您的 AD FS 部署目標，設定及設定帳戶夥伴組織和資源夥伴組織的相關資訊。  
  
## <a name="in-this-section"></a>本節內容  
  
-   [安全規劃和部署 AD FS 的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [規劃與 AD FS 1.x 的互通性](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [使用身分識別委派的時機](When-to-Use-Identity-Delegation.md)  
  
-   [在帳戶夥伴組織中部署 AD FS](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [在資源夥伴組織中部署 AD FS](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)


