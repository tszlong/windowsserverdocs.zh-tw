---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: "設計網站拓撲"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3f16b5b941ef9c3bd8f4bf742d432afc1b3f559a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-site-topology"></a>設計網站拓撲

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Directory 服務網站拓撲是邏輯表示您的實體網路。 Active Directory Domain Services (AD DS) 設計的網站拓撲涉及規劃網域控制站位置設計的網站、子網路、網站的連結，並確保有效率路由查詢和複寫流量的網站連結橋樑。  
  
設計網站拓撲可協助您有效率路由傳送 client 查詢和 Active Directory 複寫傳輸。 完善的網站拓撲可協助您的組織達成下列優點：  
  
-   最小化複寫 Active Directory 資料的費用。  
  
-   最小化維護網站拓撲所需的系統管理工作。  
  
-   排程複寫，可讓慢或撥號網路連結峰複寫 Active Directory 資料的位置。  
  
-   最佳化找出最近的資源，例如網域控制站和散發檔案系統 (DFS) 伺服器 client 電腦的能力。 這有助於降低透過速度緩慢的網路流量網路 (WAN) 連結、改善登入及登出程序和加快檔案下載作業。  
  
設計網站拓撲您開始之前，您必須了解您的實體網路結構。 此外，您必須先設計 Active Directory 邏輯結構，包括管理階層、樹系計劃，與每個樹系網域計劃。 您還必須 AD ds 完成您的網域名稱系統」(DNS) 基礎結構設計。 如需關於設計 Active Directory 邏輯結構和 DNS 基礎結構的資訊，請查看[設計邏輯結構適用於 Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx)。  
  
完成您的網站拓撲設計之後，您必須先確認您的網域控制站與 Windows Server 2008 標準、Windows Server 2008 企業版和 Windows Server 2008 Datacenter 的硬體需求。  
  
## <a name="in-this-guide"></a>本指南  
  
-   [了解 Active Directory 網站拓撲](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [收集資訊的網路](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [規劃網域控制站位置](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [建立一個網站設計](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [建立一個網站連結設計](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [建立一個網站連結橋接器設計](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [尋找 Windows Server 2008 Active Directory 網站拓撲設計的額外的資源](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


