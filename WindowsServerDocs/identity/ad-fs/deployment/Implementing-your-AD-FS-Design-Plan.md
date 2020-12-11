---
description: 深入瞭解：實施您的 AD FS 設計計畫
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: 實作您的 AD FS 設計計畫
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 9f9e4b0f4ccf51e976a3b62fb03ee37f81711346
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041086"
---
# <a name="implementing-your-ad-fs-design-plan"></a>實作您的 AD FS 設計計畫

下列環境條件和需求是 Active Directory 同盟服務 \( AD FS 設計計畫的實作為的重要因素 \) ：

-   **支援的夥伴：** 您通常會使用 AD FS 來與夥伴組織合作。 若要建立身分識別同盟，請判斷您想要形成合作關係的組織。 在基準 AD FS 部署完成後，與夥伴合作時，會牽涉到新增夥伴、刪除夥伴，以及更新夥伴資訊。 對合作關係的變更可能會因各種原因而發生。 例如，如果您的夥伴大幅變更其業務，您的 AD FS 部署可能需要合作更新，您的組織會成為較大型組織或組織同盟的一部分，或者您的組織是由不同的公司取得。 在您從多個網域同盟身分識別的任何情況下，您將需要知道 \( 您目前支援的網域夥伴， \) 以及所有代表潛在夥伴的其他網域。

-   **支援的應用程式和服務類型：** 有些應用程式和服務需要存取作業系統資源，而有些則是「宣告感知」。 請務必瞭解 AD FS 支援的應用程式和服務類型，讓您可以制定管理需求。

-   **邏輯和實體架構圖或部署拓撲：** 您將需要知道：

    -   同盟伺服器是否可在一組陣列伺服器或單一伺服器上運作。

    -   您的網路在哪裡部署防火牆和 proxy。

    -   資源的位置，以及使用者是否從組織內部、組織外部或兩者存取資源。

## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>如何使用本指南來實行您的 AD FS 設計
執行您的設計的下一個步驟是判斷每個部署工作必須執行的順序。 本指南使用檢查清單，以協助您逐步完成實作您的設計計劃所需的各種伺服器和應用程式部署工作。 您可以視需要使用父子式檢查清單來代表必須處理特定 AD FS 設計之工作的順序。

您可以使用本指南的這一節中的下列父檢查清單，熟悉用來執行組織慣用 AD FS 設計的部署工作：

-   [檢查清單：實作網頁 SSO 設計](Checklist--Implementing-a-Web-SSO-Design.md)

-   [檢查清單：實作同盟網頁 SSO 設計](Checklist--Implementing-a-Federated-Web-SSO-Design.md)
