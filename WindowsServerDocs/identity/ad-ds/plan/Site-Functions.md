---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: 站台功能
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 633c9557dffd2ce93bb913a2009a265472f4c500
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070150"
---
# <a name="site-functions"></a>站台功能

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

 Windows Server 2008 使用網站資訊有許多用途，包括路由複寫、用戶端親和性、系統磁碟區 (SYSVOL) 複寫、分散式檔案系統命名空間 (DFSN) 和服務位置。

## <a name="routing-replication"></a>路由複製
Active Directory Domain Services (AD DS) 會使用多宿主、儲存和轉寄的複寫方法。 網域控制站會將目錄變更傳達給第二個網域控制站，然後再與第三個網域控制站通訊，依此類推，直到所有網域控制站都收到變更為止。 為了達到降低複寫延遲和減少流量之間的最佳平衡，網站拓撲藉由區分網站內發生的複寫和網站之間發生的複寫，來控制 Active Directory 複寫。

在網站內，複寫會針對速度優化、資料更新觸發複寫，而且資料會在沒有資料壓縮所需的額外負荷的情況下傳送。 相反地，會壓縮網站間的複寫，以最小化透過廣域網路 (WAN) 連結的網路傳輸成本。 在網站之間進行複寫時，每個網站上每個網域的單一網域控制站都會收集並儲存目錄變更，並在排程的時間將它們與另一個網站中的網域控制站進行通訊。

## <a name="client-affinity"></a>用戶端親和性
網域控制站會使用網站資訊來通知 Active Directory 用戶端位於與用戶端最接近的網站內的網域控制站。 例如，假設西雅圖網站中的用戶端不知道其網站的關係，並與來自亞特蘭大網站的網域控制站聯繫。 根據用戶端的 IP 位址，亞特蘭大中的網域控制站會決定用戶端實際來自哪個網站，並將網站資訊傳回用戶端。 網域控制站也會通知用戶端，您所選擇的網域控制站是否與其最接近。 用戶端會快取在亞特蘭大的網域控制站所提供的網站資訊、對網站專屬服務的查詢 (SRV) 資源記錄 (網域名稱系統 (DNS) 資源記錄，以用來尋找 AD DS) 的網域控制站，進而在相同的網站中尋找網域控制站。

藉由在相同的網站中尋找網域控制站，用戶端便可避免透過 WAN 連結進行通訊。 如果沒有網域控制站位於用戶端網站，則相對於其他已連線的網站的最低成本連線通告本身 (會在沒有網域控制站的網站中， (SRV) 資源) 記錄註冊網站特定服務。 在 DNS 中發佈的網域控制站是來自網站拓撲所定義之最接近網站的網域控制站。 此程式可確保每個網站都有慣用的網域控制站來進行驗證。

如需尋找網域控制站之程式的詳細資訊，請參閱 [Active Directory 收集](/previous-versions/windows/it-pro/windows-server-2003/cc780036(v=ws.10))。

## <a name="sysvol-replication"></a>SYSVOL 複寫
SYSVOL 是檔案系統中的資料夾集合，該資料夾位於網域中的每個網域控制站上。 SYSVOL 資料夾為必須在整個網域中複寫的檔案提供預設 Active Directory 位置，包括 (Gpo) 、啟動和關閉腳本，以及登入和登出腳本的群組原則物件。  Windows Server 2008 可以使用 (FRS) 的檔案複寫服務或分散式檔案系統複寫 (DFSR) 將 SYSVOL 資料夾的變更從某個網域控制站複寫到其他網域控制站。 FRS 和 DFSR 會根據您在網站拓撲設計期間所建立的排程來複寫這些變更。

## <a name="dfsn"></a>DFSN
DFSN 會使用網站資訊，將用戶端導向至裝載網站內所要求資料的伺服器。 如果 DFSN 在與用戶端相同的網站內找不到一份資料複本，則 DFSN 會使用 AD DS 中的網站資訊，來判斷哪一個具有 DFSN 共用資料的檔案伺服器最接近用戶端。

## <a name="service-location"></a>服務位置
藉由在 AD DS 中發行服務（例如檔案和列印服務），您可以讓 Active Directory 用戶端在相同或最近的網站內找出要求的服務。 列印服務會使用儲存在 AD DS 中的 [位置] 屬性，讓使用者不需要知道確切的位置，即可依位置流覽印表機。 如需有關設計和部署列印伺服器的詳細資訊，請參閱 [設計和部署列印伺服器](/previous-versions/windows/it-pro/windows-server-2003/cc785842(v=ws.10))。
