---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: 規劃部署
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6386aac112fcf936ccdd9772e3d5566d8dd21ad8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858641"
---
# <a name="planning-your-deployment"></a>規劃部署

當您使用 Active Directory 同盟服務 \(AD FS\)來規劃跨\-的組織 \(同盟\-\) 共同作業時，請先判斷您的組織是否會裝載網路資源，以供其他組織透過網際網路存取，或是您將為組織中的員工提供 Web 資源的存取權。 這項決定會影響您部署 AD FS 的方式，而且它是規劃 AD FS 基礎結構的基礎。  
  
> [!NOTE]  
> 請確定所有合作對象清楚瞭解組織在同盟協議中所扮演的角色。  
  
針對同盟[網頁 SSO 設計](Federated-Web-SSO-Design.md)，AD FS 會使用*帳戶夥伴*\(這類詞彙，在\) 和*資源夥伴*的 AD FS 管理嵌入式\-管理單元中也稱為*識別提供者*，\(中的 AD FS 管理嵌入式管理單元\-也稱為「*信賴*憑證者」，以協助區分主控\) 帳戶夥伴 \(組織中的帳戶，這是\) 資源夥伴\-的 Web \(資源。\)  
  
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


