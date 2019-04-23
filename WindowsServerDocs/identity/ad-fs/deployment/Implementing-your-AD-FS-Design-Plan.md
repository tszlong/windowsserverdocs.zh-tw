---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: 實作您的 AD FS 設計計畫
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 31c2e048c3c125d0ea60610b049501151d7aa823
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830779"
---
# <a name="implementing-your-ad-fs-design-plan"></a>實作您的 AD FS 設計計畫

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

下列環境的條件和需求都是在 Active Directory 同盟服務的實作中的重要因素\(AD FS\)設計計劃：  
  
-   **支援的合作夥伴：** 您通常會使用夥伴組織使用 AD FS。 若要建立身分識別同盟，決定要用來形成合作關係的組織。 基準 AD FS 部署就緒之後，作業與合作夥伴需要加入夥伴、 刪除合作夥伴，並更新夥伴資訊。 合作關係的變更可能會發生各種不同的原因。 例如，您的 AD FS 部署可能需要的合作關係更新如果您的合作夥伴有重大的改變其業務、 您的組織變得較大的組織或組織的同盟的一部分或您的組織會取得由另公司。 在任何您建立同盟身分識別，從多個網域的案例中，您必須知道網域\(合作夥伴\)您目前支援和代表潛在的協力電腦的所有其他網域。  
  
-   **支援的應用程式和服務類型：** 某些應用程式和服務需要存取作業系統資源，而有些則是 「 宣告感知。 」 請務必了解應用程式和 AD FS 支援，讓您可以編寫系統管理需求的服務類型。  
  
-   **邏輯與實體架構的圖表或部署拓撲：** 您必須知道：  
  
    -   是否在一組的陣列的伺服器，或在單一伺服器上的同盟伺服器將運作。  
  
    -   其中的防火牆和 proxy 會部署您的網路。  
  
    -   資源和使用者是否存取外部組織，或兩者，在組織中的資源的位置。  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>如何實作您使用本指南的 AD FS 設計  
實作您的設計的下一個步驟是決定必須以何種順序執行每個部署工作。 本指南使用檢查清單，以協助您逐步完成實作您的設計計劃所需的各種伺服器和應用程式部署工作。 父系和子系的檢查清單可依需要來代表設計必須處理特定的 AD FS 的工作順序。  
  
使用本指南的這一節的下列父檢查清單，來熟悉實作您的組織慣用的 AD FS 設計的部署工作：  
  
-   [檢查清單：實作網頁 SSO 設計](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [檢查清單：實作同盟的網頁 SSO 設計](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
