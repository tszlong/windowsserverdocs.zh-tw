---
description: 深入瞭解：規劃部署
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: 規劃部署
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: cf87eb0407ae963d74d90696ed46b90e9e5152b7
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040406"
---
# <a name="planning-your-deployment"></a>規劃部署

當您使用 Active Directory 同盟服務 AD FS 規劃跨組織同盟型共同作業時 \- \( \- \) \( \) ，請先判斷您的組織是否會裝載 web 資源以供網際網路上的其他組織存取，或者，如果您將為組織中的員工提供 web 資源的存取權。 這個決定會影響 AD FS 的部署方式，並且是規劃 AD FS 基礎結構的基礎。

> [!NOTE]
> 請確定所有合作對象清楚瞭解組織在同盟協議中所扮演的角色。

針對同盟 [網頁 SSO 設計](Federated-Web-SSO-Design.md)，AD FS 使用帳戶夥伴等條款，也稱為「*帳戶夥伴*」（ \( AD FS 管理嵌入式管理單元中的身分 *識別提供者*），而資源夥伴亦稱為「AD FS 管理」嵌入式管理單元中的「信賴憑證者」， \- \)  \(  \- \) 可協助區分將帳戶 \( 夥伴 \) 從裝載 Web 資源夥伴之 Web 資源的組織帳戶夥伴 \- \( \) 的組織。

在 [Web SSO Design](Web-SSO-Design.md)中，組織會同時作為帳戶夥伴和資源夥伴角色，因為它提供其使用者其應用程式的存取權。

下列主題將說明部分 AD FS 夥伴組織的概念。 它們也包含《 AD FS 部署指南》中的主題連結，其中包含根據您的 AD FS 部署目標設定及設定帳戶夥伴組織和資源夥伴組織的相關資訊。

## <a name="in-this-section"></a>本節內容

-   [安全規劃和部署 AD FS 的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)

-   [規劃與 AD FS 1.x 的互通性](Planning-for-Interoperability-with-AD-FS-1.x.md)

-   [使用身分識別委派的時機](When-to-Use-Identity-Delegation.md)

-   [在帳戶夥伴組織中部署 AD FS](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)

-   [在資源夥伴組織中部署 AD FS](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)


