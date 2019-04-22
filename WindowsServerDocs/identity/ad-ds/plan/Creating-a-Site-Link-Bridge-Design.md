---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: 建立站台連結橋接器設計
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a194aa2fe2594c518d310cd86549945487d101e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813989"
---
# <a name="creating-a-site-link-bridge-design"></a>建立站台連結橋接器設計

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

站台連結橋接器連接兩個或多個站台連結，並可讓站台連結之間轉移。 在橋接器中的每個站台連結必須具有與另一個站台連結橋接器中的站台。 知識一致性檢查程式 (KCC) 會使用每個站台連結上的資訊，來計算一個站台連結中的站台與中其他站台連結橋接器的站台之間複寫的成本。 如果沒有一般站台之間的站台連結的顯示，KCC 也無法建立連接相同的站台連結橋接器的站台中的網域控制站之間的直接連線。  
  
根據預設，所有站台連結都是可轉移的。 我們建議您保留不變更的預設值啟用的轉移**橋接所有站台連結**（預設為啟用）。 不過，您必須停用**橋接所有站台連結**並完成站台連結橋接器設計，如果：  

- 不完全路由 IP 網路。 當您停用**橋接所有站台連結**，所有站台連結會視為非轉移的且您可以建立並設定站台連結橋接器物件來建立您的網路的實際路由行為的模型。  
- 您要控制複寫的 Active Directory 網域服務 (AD DS) 中所做的變更。 藉由停用**橋接所有站台連結**對於站台連結 IP 傳輸，並設定站台連結橋接器，站台連結橋接器會變成脫離網路的對等項目。 內的站台連結橋接器的所有站台連結可以路由可轉移的但不是外部站台連結橋接器路由傳送。  

如需有關如何使用 Active Directory 站台及服務嵌入式管理單元來停用**橋接所有站台連結**設定，請參閱本文[啟用或停用站台連結橋接器](https://go.microsoft.com/fwlink/?LinkId=107073)。  
  
## <a name="controlling-ad-ds-replication-flow"></a>控制 AD DS 複寫流量

兩個的情況下，您需要控制複寫資料流程的站台連結橋接器設計包括控制複寫容錯移轉，以及控制透過防火牆的複寫。  
  
### <a name="controlling-replication-failover"></a>控制複寫容錯移轉

如果貴組織擁有的中樞輪輻網路拓撲，您通常不想衛星站台，如果中樞站台中的所有網域控制站都無法建立複寫連線至其他衛星站台。 在此情況下，您必須停用**橋接所有站台連結**及建立站台連結橋接器，讓衛星站台與另一個是只有一個或兩個躍點離開衛星站台的中樞站台之間建立複寫連接。  
  
### <a name="controlling-replication-through-a-firewall"></a>控制透過防火牆的複寫

如果兩個代表兩個不同的站台的相同網域的網域控制站會特別允許彼此僅通過防火牆進行通訊，您可以停用**橋接所有站台連結**並建立的站台連結橋接器防火牆的同一邊上站台。 因此，如果您的網路分開的防火牆，我們建議您停用的站台連結轉移性和防火牆的一端上建立網路的站台連結橋接器。 管理複寫，透過防火牆的相關資訊，請參閱文章[防火牆分段之網路中的 Active Directory](https://go.microsoft.com/fwlink/?LinkId=107074)。
