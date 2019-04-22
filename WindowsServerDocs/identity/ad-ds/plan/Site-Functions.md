---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: 站台功能
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a330a9dae8ab8d3b9de5d1fff0e52060908f520
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815809"
---
# <a name="site-functions"></a>站台功能

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

 Windows Server 2008 用於許多用途，包括路由複寫、 用戶端親和性、 系統磁碟區 (SYSVOL) 的複寫、 分散式檔案系統命名空間 (DFSN)，以及服務位置的站台資訊。  
  
## <a name="routing-replication"></a>路由複寫  
Active Directory 網域服務 (AD DS) 會使用多重主機、 儲存和轉寄的複寫方法。 網域控制站進行通訊的第二個網域控制站，會接著傳送至第三個，依此類推，直到所有網域控制站都收到變更的目錄變更。 若要達到最佳平衡減少複寫延遲，並減少流量，站台拓撲，請以區別站台內，就會發生的複寫和站台之間，就會發生的複寫控制 Active Directory 複寫。  
  
站台內複寫最適合的速度、 資料更新觸發程序複寫，以及資料會傳送，而不需資料壓縮的額外負荷。 相反地，站台間複寫是經過壓縮以減少傳輸的成本透過廣域網路 (wan) 連結。 站台間複寫時，每個網域的每個站台的單一網域控制站會收集和儲存目錄變更並與它們通訊在排定的時間，另一個站台中的網域控制站。  
  
## <a name="client-affinity"></a>用戶端親和性  
網域控制站以通知出現在最接近的站台與用戶端的網域控制站的 Active Directory 用戶端使用站台資訊。 比方說，請考慮在西雅圖站台，並不知道其站台關聯，並連絡網域控制站從亞特蘭大站台的用戶端。 根據用戶端的 IP 位址，亞特蘭大的網域控制站會決定哪些用戶端實際上是來自，並將站台資訊傳送回用戶端的站台。 用戶端選擇的網域控制站是否為最接近一個它，也會通知網域控制站。 用戶端會快取提供亞特蘭大 」、 查詢站台特定的服務 (SRV) 資源記錄 （用來尋找適用於 AD DS 的網域控制站網域名稱系統 (DNS) 資源記錄） 的網域控制站的站台資訊，並藉此尋找網域在相同的站台內的控制站。  
  
尋找網域控制站位於相同站台，用戶端會避免透過 WAN 連結的通訊。 如果沒有網域控制站位於用戶端站台，具有最低的成本連線相對於其他連線的站台的網域控制站就會將本身 （在 DNS 中註冊站台特定的服務 (SRV) 資源記錄） 通告所沒有的網站網域控制站。 在 DNS 中發佈的網域控制站是從最接近的站台的站台拓撲所定義。 此程序可確保每個站台已驗證的慣用的網域控制站。  
  
尋找網域控制站的程序的相關資訊，請參閱 Active Directory 集合 ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626))。  
  
## <a name="sysvol-replication"></a>SYSVOL 複寫  
SYSVOL 是存在於網域中的每個網域控制站的檔案系統中的資料夾的集合。 SYSVOL 資料夾提供的預設 Active Directory 位置必須複寫整個網域，包括群組原則物件 (Gpo)、 啟動和關機指令碼，以及登入和登出指令碼的檔案。  Windows Server 2008 可以使用檔案複寫服務 (FRS) 或分散式檔案系統複寫 (DFSR) 來複寫 SYSVOL 資料夾從一個網域控制站，其他網域控制站進行的變更。 FRS 和 DFSR 複寫這些變更，根據您在您的站台拓撲設計期間建立的排程。  
  
## <a name="dfsn"></a>DFSN  
DFSN 會直接裝載所要求的資料內的站台伺服器的用戶端使用站台資訊。 如果 DFSN 與用戶端位於相同站台內找不到資料的複本，DFSN 會使用在 AD DS 以判斷哪一個檔案伺服器並包含 DFSN 共用資料的站台資訊是最接近用戶端。  
  
## <a name="service-location"></a>服務位置  
在 AD DS 中發佈服務，例如檔案和列印服務，您就可以允許 Active Directory 用戶端找到要求的服務在相同或最接近的站台。 列印服務會使用儲存在 AD DS 之 location 屬性，讓使用者瀏覽依位置的印表機，而不需要知道確切的位置。 如需有關設計和部署列印伺服器的詳細資訊，請參閱設計和部署列印伺服器 ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041))。  
  


