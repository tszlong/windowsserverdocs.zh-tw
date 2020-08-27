---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: 收集網路資訊
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 3c7e7a0a3d193128ecc2186c15eb8f9c0b9d93a9
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941198"
---
# <a name="collecting-network-information"></a>收集網路資訊

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory Domain Services (AD DS) 中設計有效的網站拓撲的第一步，就是洽詢您組織的網路群組來收集資訊，並定期與您的實體網路拓撲進行通訊。

## <a name="creating-a-location-map"></a>建立位置對應

建立位置對應，以代表您組織的實體網路基礎結構。 在位置對應中，找出包含內部連線的電腦群組的地理位置（每秒 10 mb） (Mbps) 或更高的 (區域網路 (LAN) 速度或更好的) 。

## <a name="listing-communication-links-and-available-bandwidth"></a>列出通訊連結和可用頻寬

建立位置對應之後，記錄通訊連結的類型、其連結速度，以及每個位置之間的可用頻寬。 從網路群組取得廣域網路 (WAN) 拓撲。 如需常見的 WAN 線路類型及其頻寬的清單，請參閱 [建立網站連結設計](../../ad-ds/plan/Creating-a-Site-Link-Design.md)中的「判斷成本」一節。 您稍後會在網站拓撲設計程式中需要這項資訊來建立網站連結。

頻寬是指您在指定的時間內，可以跨通道傳輸的資料量。 可用的頻寬是指 AD DS 實際可使用的頻寬量。 您可以從網路群組取得可用的頻寬資訊，也可以使用通訊協定分析器（例如網路監視器）來分析每個連結上的流量。 如需安裝網路監視器的詳細資訊，請參閱 [監視網路流量](/previous-versions/windows/it-pro/windows-server-2003/cc783075(v=ws.10))的文章。

記錄每個位置以及連結至該位置的其他位置。 此外，請記錄通訊連結的類型及其可用的頻寬。 如需協助您列出通訊連結和可用頻寬的工作表，請參閱 [適用于 Windows Server 2003 部署套件的工作輔助程式](https://microsoft.com/download/details.aspx?id=9608)、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，以及開啟「地理位置和通訊連結」 ( # A1) 。

## <a name="listing-ip-subnets-within-each-location"></a>列出每個位置內的 IP 子網

記錄通訊連結和每個位置之間的可用頻寬之後，請在每個位置中記錄 IP 子網。 如果您還不知道每個位置內的子網路遮罩和 IP 位址，請參閱您的網路群組。

AD DS 將工作站的 IP 位址與每個網站相關聯的子網進行比較，以將工作站與網站產生關聯。 當您將網域控制站新增至網域時，AD DS 也會檢查其 IP 位址，並將它們放在最適當的網站。

如需協助您列出每個位置內 IP 子網的工作表，請參閱 [適用于 Windows Server 2003 部署套件的工作輔助工具](https://microsoft.com/download/details.aspx?id=9608)、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，以及開啟「位置和子網」 ( # A1) 。

> [!NOTE]
> 除了 IP 第4版 (IPv4) 位址，Windows Server 也支援 IP 第6版 (IPv6) 子網首碼。 如需協助您列出 IPv6 子網首碼的工作表，請參閱 [附錄 a：位置和子網首碼](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md)。

## <a name="listing-domains-and-number-of-users-for-each-location"></a>列出每個位置的網域和使用者數目

位置中所代表每個地區網域的使用者數目是決定區域網域控制站和通用類別目錄伺服器位置的因素之一，也就是網站拓撲設計程式中的下一個步驟。 例如，規劃將區域性網域控制站放在包含超過100個地區網域使用者的位置，讓他們仍能在 WAN 連結失敗時登入網域。

記錄位置、每個位置中表示的網域，以及每個位置所代表每個網域的使用者數目。 如需協助您列出網域以及每個位置中表示的使用者數目的工作表，請參閱 [Windows Server 2003 部署套件的工作輔助](https://microsoft.com/download/details.aspx?id=9608)工具、下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，以及開啟「每個位置的網域和使用者」 ( # A1) 。
