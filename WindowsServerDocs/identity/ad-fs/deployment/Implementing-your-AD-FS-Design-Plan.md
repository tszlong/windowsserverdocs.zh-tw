---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: 實作您的 AD FS 設計計畫
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6306b87dd06774bfde5ffc3ff98818d47d0c858f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408375"
---
# <a name="implementing-your-ad-fs-design-plan"></a>實作您的 AD FS 設計計畫

下列環境條件和需求是執行 Active Directory 同盟服務 \(AD FS\) 設計計畫中的重要因素：  
  
-   **支援的合作夥伴：** 您通常會使用 AD FS 來與夥伴組織合作。 若要建立識別身分同盟，請決定您想要形成合作關係的組織。 在基準 AD FS 部署就緒之後，與合作夥伴合作的作業包括新增夥伴、刪除夥伴，以及更新合作夥伴資訊。 對合作關係的變更可能會因各種原因而發生。 例如，您的 AD FS 部署可能需要合作關係更新，如果您的合作夥伴大幅變更其業務、您的組織會成為較大型組織或組織同盟的一部分，或者您的組織是由不同的來取得家. 在您將多個網域中的身分識別同盟的任何情況下，您都必須知道您目前支援的網域 \(合作夥伴\)，以及代表潛在合作夥伴的所有其他網域。  
  
-   **支援的應用程式和服務類型：** 有些應用程式和服務需要存取作業系統資源，有些則是「宣告感知」。 請務必瞭解 AD FS 支援的應用程式和服務類型，讓您能夠制訂管理需求。  
  
-   **邏輯和實體架構圖表或部署拓朴：** 您將需要知道：  
  
    -   同盟伺服器是否會在一組陣列伺服器或單一伺服器上運作。  
  
    -   您的網路部署防火牆和 proxy 的位置。  
  
    -   資源的位置，以及使用者是否要從組織內部、組織外部或兩者存取資源。  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>如何使用本指南來執行您的 AD FS 設計  
執行設計的下一個步驟是判斷每個部署工作必須執行的順序。 本指南使用檢查清單，以協助您逐步完成實作您的設計計劃所需的各種伺服器和應用程式部署工作。 父和子檢查清單會在必要時使用，以代表特定 AD FS 設計的工作必須處理的順序。  
  
使用本指南的這一節中的下列父檢查清單，以熟悉用來執行貴組織慣用 AD FS 設計的部署工作：  
  
-   [檢查清單：執行網頁 SSO 設計](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [檢查清單：執行同盟網頁 SSO 設計](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
