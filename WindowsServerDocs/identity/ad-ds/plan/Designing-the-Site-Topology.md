---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: 設計站台拓撲
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e1d9323ceda478369973f959687d46c9ca3cb88f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888489"
---
# <a name="designing-the-site-topology"></a>設計站台拓撲

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

目錄服務的站台拓樸是您的實體網路的邏輯表示法。 設計 Active Directory 網域服務 (AD DS) 站台拓樸，牽涉到規劃設置網域控制站和設計站台、 子網路、 站台連結及站台連結橋接器，以確保有效率的查詢和複寫流量的路由。  
  
設計站台拓撲，可協助您有效率地路由傳送用戶端查詢和 Active Directory 複寫流量。 設計良好的站台拓樸可協助您的組織獲得下列優點：  
  
-   複寫 Active Directory 資料的成本降至最低。  
  
-   最小化，才能維護站台拓撲的系統管理工作。  
  
-   啟用慢速或撥號網路連結，來複寫 Active Directory 資料，在離峰時間的位置的複寫排定在離。  
  
-   最佳化用戶端電腦能夠找出最接近的資源，例如網域控制站和分散式檔案系統 (DFS) 伺服器。 有助於減少網路流量，透過緩慢的廣域網路 (WAN) 連線的網路、 改進登入和登出程序，和加速檔案下載作業。  
  
在開始設計您的站台拓撲之前，您必須了解您實體網路的結構。 此外，您必須先設計 Active Directory 邏輯結構，包括系統管理階層、 樹系方案，以及每個樹系的網域計劃。 您也必須完成您的網域名稱系統 (DNS) 基礎結構設計，適用於 AD DS。 如需設計您的邏輯結構，Active Directory 和 DNS 基礎結構的詳細資訊，請參閱 <<c0> [ 設計邏輯結構的 Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx)。  
  
完成您的站台拓撲設計之後，您必須確認您的網域控制站符合 Windows Server 2008 Standard、 Windows Server 2008 Enterprise 和 Windows Server 2008 Datacenter 的硬體需求。  
  
## <a name="in-this-guide"></a>本指南內容  
  
-   [了解 Active Directory 網站拓撲](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [收集網路資訊](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [規劃設置網域控制站](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [建立站台設計](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [建立站台連結設計](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [建立站台連結橋接器設計](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [尋找 Windows Server 2008 Active Directory 站台拓撲設計的其他資源](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


