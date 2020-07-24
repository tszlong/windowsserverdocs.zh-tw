---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: 站台功能
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 70dc69812ae6806c58affdd3961c3ec8f9afd0bc
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963540"
---
# <a name="site-functions"></a>站台功能

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

 Windows Server 2008 使用的網站資訊有許多用途，包括路由複寫、用戶端親和性、系統磁碟區（SYSVOL）複寫、分散式檔案系統命名空間（DFSN）和服務位置。

## <a name="routing-replication"></a>路由複寫
Active Directory Domain Services （AD DS）使用複寫的多宿主、儲存後轉送方法。 網域控制站會將目錄變更與第二個網域控制站通訊，然後在所有網域控制站都收到變更之前進行通訊，依此類推。 為了達到降低複寫延遲和減少流量之間的最佳平衡，網站拓朴會藉由區分在網站內發生的複寫和網站之間發生的複寫，來控制 Active Directory 複寫。

在網站內，複寫已針對速度、資料更新觸發複寫進行優化，而且資料會在沒有資料壓縮所需的額外負荷下傳送。 相反地，會壓縮網站之間的複寫，以將透過廣域網路（WAN）連結傳輸的成本降至最低。 在網站之間進行複寫時，每個網站上每個網域的單一網域控制站會收集並儲存目錄變更，並在排定的時間將它們與另一個網站中的網域控制站進行通訊。

## <a name="client-affinity"></a>用戶端親和性
網域控制站會使用網站資訊來通知 Active Directory 用戶端與用戶端位於最接近的網站中的網域控制站。 例如，假設西雅圖網站中的用戶端不知道其網站關係，並與亞特蘭大網站的網域控制站聯繫。 根據用戶端的 IP 位址，亞特蘭大中的網域控制站會決定用戶端實際來自哪個網站，並將網站資訊傳回給用戶端。 網域控制站也會通知用戶端，選擇的網域控制站是否與它最接近。 用戶端會快取亞特蘭大中的網域控制站所提供的網站資訊、網站特定服務（SRV）資源記錄的查詢（用來尋找 AD DS 的網域控制站的網域名稱系統（DNS）資源記錄），進而尋找相同網站內的網域控制站。

藉由尋找相同網站中的網域控制站，用戶端可避免透過 WAN 連結進行通訊。 如果在用戶端網站上找不到網域控制站，則與其他連線的網站相對的最低成本連接的網域控制站會在沒有網域控制站的網站中，向其註冊網站特定服務（SRV）資源記錄。 在 DNS 中發佈的網域控制站，是來自網站拓撲所定義的最接近網站。 此程式可確保每個網站都有慣用的網域控制站來進行驗證。

如需尋找網域控制站之程式的詳細資訊，請參閱[Active Directory 集合](/previous-versions/windows/it-pro/windows-server-2003/cc780036(v=ws.10))。

## <a name="sysvol-replication"></a>SYSVOL 複寫
SYSVOL 是檔案系統中，位於網域中每個網域控制站上的資料夾集合。 SYSVOL 資料夾會針對必須在整個網域中複寫的檔案提供預設 Active Directory 位置，包括群組原則物件（Gpo）、啟動和關機腳本，以及登入和登出腳本。  Windows Server 2008 可以使用檔案複寫服務（FRS）或分散式檔案系統複寫（DFSR），將 SYSVOL 資料夾所做的變更，從一個網域控制站複寫到其他網域控制站。 FRS 和 DFSR 會根據您在網站拓撲設計期間所建立的排程來複寫這些變更。

## <a name="dfsn"></a>DFSN
DFSN 會使用網站資訊，將用戶端導向至裝載網站中所要求資料的伺服器。 如果 DFSN 在與用戶端相同的網站中找不到資料的複本，DFSN 會使用 AD DS 中的網站資訊來判斷哪個檔案伺服器的 DFSN 共用資料最接近用戶端。

## <a name="service-location"></a>服務位置
藉由在 AD DS 中發佈檔案和列印服務等服務，可讓 Active Directory 用戶端在相同或最接近的網站中找到要求的服務。 列印服務會使用儲存在 AD DS 中的 location 屬性，讓使用者能夠依位置流覽印表機，而不需要知道它們的確切位置。 如需設計和部署列印伺服器的詳細資訊，請參閱[設計和部署列印伺服器](/previous-versions/windows/it-pro/windows-server-2003/cc785842(v=ws.10))。
