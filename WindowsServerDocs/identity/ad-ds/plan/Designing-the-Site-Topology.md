---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: 設計站台拓撲
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e7b6267946217d5c5fb57496eb6bf54911b61e8a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822593"
---
# <a name="designing-the-site-topology"></a>設計站台拓撲

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

目錄服務網站拓撲是實體網路的邏輯標記法。 設計 Active Directory Domain Services （AD DS）的網站拓撲牽涉到規劃網域控制站放置和設計網站、子網、站台連結和站台連結橋，以確保查詢和複寫流量的路由有效。  
  
設計網站拓朴可協助您有效率地路由傳送用戶端查詢和 Active Directory 複寫流量。 設計完善的網站拓撲可協助您的組織達成下列優點：  
  
-   將複寫 Active Directory 資料的成本降至最低。  
  
-   將維護網站拓撲所需的管理工作降至最低。  
  
-   排程複寫，讓具有慢速或撥號網路連結的位置能夠在離峰時段複寫 Active Directory 資料。  
  
-   將用戶端電腦的功能優化，以找出最接近的資源，例如網域控制站和分散式檔案系統（DFS）伺服器。 這有助於減少透過低速廣域網路（WAN）連結的網路流量、改善登入和登出程式，並加快檔案下載作業的速度。  
  
開始設計您的網站拓撲之前，您必須先瞭解您的實體網路結構。 此外，您必須先設計 Active Directory 邏輯結構，包括每個樹系的系統管理階層、樹系計畫和網域方案。 您也必須完成 AD DS 的網域名稱系統（DNS）基礎結構設計。 如需設計 Active Directory 邏輯結構和 DNS 基礎結構的詳細資訊，請參閱[設計 Windows Server 2008 的邏輯結構 AD DS](https://technet.microsoft.com/library/cc770806.aspx)。  
  
完成網站拓朴設計之後，您必須確認您的網域控制站符合 Windows Server 2008 Standard、Windows Server 2008 Enterprise 和 Windows Server 2008 Datacenter 的硬體需求。  
  
## <a name="in-this-guide"></a>在本指南中  
  
-   [了解 Active Directory 站台拓撲](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [收集網路資訊](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [規劃設置網域控制站](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [建立站台設計](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [建立站台連結設計](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [建立站台連結橋接器設計](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [尋找 Windows Server 2008 Active Directory 網站拓朴設計的其他資源](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


