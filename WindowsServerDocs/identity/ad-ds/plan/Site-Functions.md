---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: "台功能"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f40122d84185c69fa19f5158c2b1c370ec1bfe82
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="site-functions"></a>台功能

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

 Windows Server 2008 許多用途，包括路由複寫、 client 相關性、 系統磁碟區 (SYSVOL) 複寫、 散發檔案系統命名空間 (DFSN)，以及位置服務使用資訊的網站。  
  
## <a name="routing-replication"></a>尚複寫  
Active Directory Domain Services (AD DS) 使用多主機、 儲存轉送複寫的方法。 網域控制站通訊的第二個網域控制站，然後通訊第三方，等等，以之前所有網域控制站都收到變更 directory 變更。 若要達到最佳平衡縮短複寫延遲和降低流量，台拓撲控制複寫 Active Directory 區分在網站中發生的複製和複寫的網站。  
  
在網站中複寫最適合︰ 複寫 speeddata 更新觸發程序，並資料傳送不資料壓縮所需的費用。 反而，之間網站複寫壓縮透過寬區域 (wan) 連結減少的據傳輸費用。 之間網站複寫時，每個網域在每個網站單一網域控制站會收集 directory 變更會儲存並傳送另一個網站的網域控制站排程的時間。  
  
## <a name="client-affinity"></a>Client 相關性  
網域控制站使用通知 Active Directory 戶端網域控制站出現在接近網站的相關資訊的網站。 例如，請考慮 client 西雅圖網站，並不知道其網站聯盟連絡人網域控制站的亞特蘭大網站中。 根據 client 的 IP 位址，網域控制站亞特蘭大判斷 client 實際上是來自和網站會將資訊傳送到 client 的網站。 網域控制站也會通知 client 選擇的網域控制站是否是最近一它。 Client 快取提供查詢的特定網站服務 (SRV) 資源筆 （網域名稱系統 」 (DNS) 資源的記錄用來尋找網域控制站 AD ds），亞特蘭大網域控制站的網站資訊，並藉此會尋找網域控制站在相同的網站。  
  
尋找網域控制站在相同的網站，來 client 可透過 WAN 連結避免通訊。 如果不網域控制站位於 client 網站，網域控制站的其他連接網站和目的地的相對的最低成本連接通知 （暫存特定網站服務 (SRV) 資源記錄器 dns） 本身網域控制站不具有網站中。 發行 dns 網域控制站是從接近網站的網站拓撲所定義。 此程序可以確保每個網站的已驗證的慣用的網域控制站。  
  
尋找網域控制站的處理程序的相關詳細資訊，會看到 Active Directory 集合 ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626))。  
  
## <a name="sysvol-replication"></a>SYSVOL 複寫  
SYSVOL 是一組資料夾集合位於網域中的每個網域控制站檔案系統。 SYSVOL 資料夾提供必須複寫網域，包括群組原則物件 (Gpo)、 開機和關機及登入及登出指令碼的檔案的預設 Active Directory 位置。  Windows Server 2008 可以使用的檔案複製服務 (FRS) 或散發檔案系統複寫 (DFSR) 複寫 SYSVOL 資料夾的一個其他網域控制站的網域控制站所做的變更。 FRS 與 DFSR 複寫根據您在網站拓撲設計建立排程這些變更。  
  
## <a name="dfsn"></a>DFSN  
DFSN 直接 client 伺服器裝載要求在網站中的資料用於資訊的網站。 如果 DFSN 相同網站中找不到資料的複本，DFSN 使用網站中的資訊以判斷您的檔案伺服器的 DFSN 共用資料 AD DS 是最近的。  
  
## <a name="service-location"></a>位置服務  
發表服務，例如檔案和和列印服務中 AD DS，您可以允許 Active Directory 戶端以尋找最近的網站或中相同所要求的服務。 列印服務會使用儲存在 AD DS 位置屬性，讓瀏覽，位置，而不知道其允許使用準確位置的使用者。 如需有關設計和部署列印伺服器，查看設計和部署列印伺服器 ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041))。  
  


