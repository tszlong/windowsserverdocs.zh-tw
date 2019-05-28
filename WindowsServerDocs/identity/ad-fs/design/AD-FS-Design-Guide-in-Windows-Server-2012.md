---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Windows Server 2012 中的 AD FS 設計指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3f2a6df6a9c9a5cbdfa9c64bc6521e92f4982a15
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191728"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Windows Server 中的 AD FS 設計指南 


  
> [!NOTE]  
> 如需如何在 Windows Server 2012 R2 部署 AD FS 的詳細資訊，請參閱[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)。  
  
您可以使用 Active Directory® Federation Services \(AD FS\) Windows Server® 2012年作業系統中的同盟服務提供者角色，才能順暢地驗證您的使用者之任何 web\-為基礎的服務或位於資源夥伴組織，而不需要系統管理員建立或維護外部信任或樹系信任的網路的兩個組織，並不需要使用者登入第二個時間之間的應用程式。 向一個網路存取其他網路中的資源時的程序 — 使用者重複登入動作的負擔，又稱為單一登\-上\(SSO\)。  
  
## <a name="about-this-guide"></a>關於本指南  
本指南提供建議，協助您規劃新部署的 AD FS 中，根據貴組織的需求\(也稱為本指南中的部署目標\)和您想要建立的特定設計。 本指南的使用對象為基礎結構專家或系統架構設計人員。 它反白顯示您的主要決策點，當您在規劃 AD FS 部署。 在閱讀本指南之前，您應該充分了解 AD FS 功能等級上運作的方式。 您也應該充分了解組織的需求，將會反映在您的 AD FS 設計。  
  
本指南說明一組的三個主要 AD FS 設計為基礎的部署目標，並協助您決定最適當的設計，為您的環境。 您可以使用這些的部署目標，來產生下列完整的 AD FS 設計或自訂的設計符合您環境的需求之一：  
  
-   同盟網頁 SSO，來支援商務\-要\-商務\(B2B\)案例以及支援含獨立樹系之業務單位之間的共同作業  
  
-   Web SSO，可支援企業中的客戶應用程式存取權\-要\-消費者\(B2C\)案例  
  
針對每一個設計，您將發現適用於蒐集與您的環境有關之必要資料的指導方針。 您接著可以使用這些指導方針來規劃和設計您的 AD FS 部署。 在閱讀本指南並完成蒐集、 記錄及對應組織的需求之後，就會開始部署使用中的指導方針的 AD FS 所需的資訊[Windows Server 2012 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>本指南內容  
  
-   [識別您的 AD FS 部署目標](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [將您的部署目標對應至 AD FS 設計](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [決定您的 AD FS 部署拓撲](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [規劃您的部署](Planning-Your-Deployment.md)  
  
-   [規劃同盟伺服器的位置](Planning-Federation-Server-Placement.md)  
  
-   [規劃同盟伺服器 Proxy 的位置](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [規劃 AD FS 伺服器容量](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [附錄 A：檢閱 AD FS 需求](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

