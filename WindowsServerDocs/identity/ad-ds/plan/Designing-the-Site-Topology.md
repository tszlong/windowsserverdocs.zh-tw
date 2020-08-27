---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: 設計站台拓撲
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: dac3976a71b27f75cee5a2d4640282819bdfe75e
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939438"
---
# <a name="designing-the-site-topology"></a>設計站台拓撲

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

目錄服務網站拓撲是實體網路的邏輯標記法。 設計 Active Directory Domain Services (AD DS) 的網站拓朴，包括規劃網域控制站放置和設計網站、子網、網站連結和站台連結橋接器，以確保查詢和複寫流量的有效路由。

設計網站拓撲可協助您有效率地路由傳送用戶端查詢及 Active Directory 複寫流量。 設計完善的網站拓撲可協助您的組織達成下列優點：

-   將複寫 Active Directory 資料的成本降至最低。

-   將維護網站拓撲所需的管理工作降至最低。

-   排程在離峰時段，可讓具有慢速或撥號網路連結的位置複寫 Active Directory 資料的複寫。

-   將用戶端電腦的能力優化，以找出最接近的資源，例如網域控制站和分散式檔案系統 (DFS) 伺服器。 這有助於減少網路流量過慢的廣域網路 (WAN) 連結、改善登入和登出程式，以及加速檔案下載作業。

開始設計您的網站拓撲之前，您必須先瞭解您的實體網路結構。 此外，您必須先設計 Active Directory 邏輯結構，包括每個樹系的系統管理階層、樹系方案和網域方案。 您也必須為 AD DS 完成網域名稱系統 (DNS) 基礎結構設計。 如需設計 Active Directory 邏輯結構和 DNS 基礎結構的詳細資訊，請參閱 [設計 Windows Server 2008 的邏輯結構 AD DS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770806(v=ws.10))。

完成網站拓撲設計之後，您必須確認您的網域控制站符合 Windows Server 2008 Standard、Windows Server 2008 Enterprise 和 Windows Server 2008 Datacenter 的硬體需求。

## <a name="in-this-guide"></a>本指南內容

-   [了解 Active Directory 站台拓撲](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)

-   [收集網路資訊](../../ad-ds/plan/Collecting-Network-Information.md)

-   [規劃設置網域控制站](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)

-   [建立站台設計](../../ad-ds/plan/Creating-a-Site-Design.md)

-   [建立站台連結設計](../../ad-ds/plan/Creating-a-Site-Link-Design.md)

-   [建立站台連結橋接器設計](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)

-   [尋找 Windows Server 2008 的其他資源 Active Directory 網站拓撲設計](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)

