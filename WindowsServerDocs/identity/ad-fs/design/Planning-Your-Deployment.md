---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: 規劃您的部署
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5c7cec9ad92605f3dc98f8ce8fb7853a7ae61299
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863689"
---
# <a name="planning-your-deployment"></a>規劃您的部署

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

當您在規劃跨\-組織\(同盟\-基礎\)共同作業使用 Active Directory Federation Services \(AD FS\)，先判斷您的組織將裝載在網際網路上的其他組織存取 Web 資源或如果您要為員工提供 Web 資源的存取權，您組織中。 這個決定會影響您如何部署 AD FS 中，而且它是在您的 AD FS 基礎結構的規劃。  
  
> [!NOTE]  
> 請確定所有合作對象清楚瞭解組織在同盟協議中所扮演的角色。  
  
針對[Federated Web SSO Design](Federated-Web-SSO-Design.md)，例如 AD FS 使用的詞彙*帳戶夥伴*\(也稱為*身分識別提供者*中 AD FS 管理嵌入式管理單元\-中\)並*資源夥伴*\(也稱為*信賴憑證者的合作對象*AD FS 管理嵌入式管理單元中\-中\)至協助區分裝載帳戶的組織\(帳戶夥伴\)裝載網站的組織\-基礎資源\(資源夥伴\)。  
  
在 [Web SSO Design](Web-SSO-Design.md)中，組織會同時作為帳戶夥伴和資源夥伴角色，因為它提供其使用者其應用程式的存取權。  
  
下列主題將說明 AD FS 的一些合作夥伴組織的概念。 當中也包含 AD FS 部署指南中包含安裝和設定帳戶夥伴組織和資源夥伴組織，根據您的 AD FS 部署目標的相關資訊的主題連結。  
  
## <a name="in-this-section"></a>本節內容  
  
-   [安全規劃和部署的 AD FS 的最佳作法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [規劃互通性與 AD FS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [使用身分識別委派的時機](When-to-Use-Identity-Delegation.md)  
  
-   [帳戶夥伴組織中部署 AD FS](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [資源夥伴組織中部署 AD FS](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)


