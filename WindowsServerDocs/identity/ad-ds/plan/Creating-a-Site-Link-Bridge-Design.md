---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: 建立站台連結橋接器設計
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 79e91481c357d05617ee0ddc716e2bf6e90b8b27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408963"
---
# <a name="creating-a-site-link-bridge-design"></a>建立站台連結橋接器設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

站台連結橋會連接兩個或多個站台連結，並啟用站台連結之間的傳遞。 橋接器中的每個站台連結都必須具有與橋接器中另一個站台連結相同的網站。 知識一致性檢查程式（KCC）會使用每個站台連結上的資訊，來計算一個站台連結中的網站與橋接器的另一個站台連結中的網站之間的複寫成本。 如果沒有在站台連結之間出現一般網站，則 KCC 也無法在相同站台連結橋連線的網站中的網域控制站之間建立直接連線。  
  
根據預設，所有站台連結都是可轉移的。 我們建議您不要變更 [**橋接所有站台連結**] 的預設值（預設為啟用），藉以保持啟用傳遞。 不過，在下列情況下，您必須停用**橋接所有網站連結**並完成站台連結橋設計：  

- 您的 IP 網路未完全路由。 當您停用**橋接所有站台連結**時，所有站台連結都會被視為非轉移，而您可以建立並設定站台連結橋物件，以模型出網路的實際路由行為。  
- 您必須控制在 Active Directory Domain Services （AD DS）中所做變更的複寫流程。 藉由停用 [橋接] 站台連結 IP 傳輸和設定站台連結橋的**所有網站**連結，站台連結橋就等同于脫離的網路。 站台連結橋接中的所有站台連結都可以進行可轉移的路由，但不會在站台連結橋的外部路由。  

如需如何使用 [Active Directory 的網站和服務] 嵌入式管理單元來停用 [**橋接所有站台連結**] 設定的詳細資訊，請參閱[啟用或停用站台連結橋](https://go.microsoft.com/fwlink/?LinkId=107073)文章。  
  
## <a name="controlling-ad-ds-replication-flow"></a>控制 AD DS 複寫流程

在兩種情況下，您需要站台連結橋設計來控制複寫流程，包括控制複寫容錯移轉，以及透過防火牆控制複寫。  
  
### <a name="controlling-replication-failover"></a>控制複寫容錯移轉

如果您的組織有中樞和輪輻網路拓撲，則當中樞網站中的所有網域控制站都失敗時，您通常不會想要讓衛星網站建立與其他附屬網站的複寫連線。 在這種情況下，您必須停用 [**橋接所有站台連結**] 和 [建立站台連結橋]，讓複寫連線在附屬網站與另一個中樞網站之間建立，而這只是一個或兩個來自附屬網站的躍點。  
  
### <a name="controlling-replication-through-a-firewall"></a>控制透過防火牆的複寫

如果在兩個不同的網站中代表相同網域的兩個網域控制站特別允許透過防火牆彼此通訊，您可以停用**橋接所有站台連結**，並在相同的那一邊建立網站的站台連結橋防火牆. 因此，如果您的網路是以防火牆分隔，建議您停用站台連結的轉移，並在防火牆的一端建立網路的站台連結橋。 如需透過防火牆管理複寫的相關資訊，請參閱[在防火牆分割的網路中 Active Directory](https://go.microsoft.com/fwlink/?LinkId=107074)一文。
