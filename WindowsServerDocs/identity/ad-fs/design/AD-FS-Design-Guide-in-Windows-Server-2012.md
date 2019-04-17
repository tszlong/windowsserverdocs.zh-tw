---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: "Windows Server 2012 中的 AD FS 設計指南"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e660c1dabcc5a683fa74068ea148fd4efbeee569
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012"></a>Windows Server 2012 中的 AD FS 設計指南

>適用於：Windows Server 2012
  
> [!NOTE]  
> 了解如何在 Windows Server 2012 R2 AD FS 的部署的資訊，請查看[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)。  
  
您可以使用 Windows Server® 2012 年作業系統同盟服務提供者的角色 Active Directory（）同盟服務 \(AD FS\) 順暢地進行驗證使用者任何 Web\ 為基礎的服務或位於資源合作夥伴組織，而不需要系統管理員建立，或維持外部信任或網路的兩個組織，而不需要的使用者來登入第二次之間的樹系信任的應用程式。 網路存取的其他網路資源時進行驗證的程序，重複登入動作使用者的負擔 — 稱為單一 sign\ 上 \(SSO\)。  
  
## <a name="about-this-guide"></a>有關本指南  
本指南建議，可協助您計畫 AD FS，根據您的組織需求部署新 \（也稱為為部署 goals\ 本指南）和您想要建立特定的設計。 本指南被針對使用的基礎結構專員或系統架構。 它會反白顯示您的主要決策點為您計劃 AD FS 部署。 本指南朗讀時之前，您應該會有深入了解 AD FS 上功能的層級的運作方式。 您也應該有您 AD FS 設計深入了解會反映出剛剛組織需求。  
  
本指南告訴您的部署目標三個主要 AD FS 設計為基礎的設定，可協助您判斷最適合用於您的環境設計。 您可以使用下列的完整 AD FS 設計或符合您的環境需求自訂設計的其中一個表單這些部署目標：  
  
-   聯盟的網路 SSO business\ to\ 商務 \(B2B\) 案例的支援，並支援與獨立樹系的業務單位之間共同作業  
  
-   Web SSO business\ to\ 消費者 \(B2C\) 案例中支援客戶存取應用程式  
  
針對每個設計，您將會收集關於您的環境所需的資料尋找指導方針。 您再可以使用下列指導方針操作計劃及設計 AD FS 部署。 朗讀本指南後，當您完成收集、文件，以及對應您組織的需求，您將會有開始部署 AD FS 使用中的指導所需的資訊[Windows Server 2012 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md)。  
  
## <a name="in-this-guide"></a>本指南  
  
-   [找出您 AD FS 部署務目標](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [將您的部署目標對應至 AD FS 設計](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [判斷您 AD FS 部署拓撲](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [規劃部署](Planning-Your-Deployment.md)  
  
-   [規劃伺服器聯盟的位置](Planning-Federation-Server-Placement.md)  
  
-   [規劃聯盟 Proxy 伺服器的位置](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [AD FS 伺服器容量的計劃](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [答附錄審查 AD FS 需求](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

