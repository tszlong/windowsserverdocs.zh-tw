---
title: 使用 DNS 原則 Split-Brain DNS Active Directory 中
description: 您可以使用此主題利用流量 DNS split-brain Active Directory 部署的原則的管理功能整合在 Windows Server 2016 DNS 區域。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d294fcad3b48c8698dffd93e94f6ef7ffc681ea2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>使用 DNS 原則 Split-Brain DNS Active Directory 中

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題的 Active Directory 部署 split\ 蛋整合 DNS 利用 DNS 原則的資料傳輸管理功能在 Windows Server 2016 的區域。

在 Windows Server 2016 DNS 原則的支援延伸到 Active Directory 整合 DNS 區域。 Active Directory 整合提供 multi\ 主機可用性功能 DNS 伺服器。 

之前，此案例，您必須 DNS 系統管理員，維護兩個不同的 DNS 伺服器，每個設定的使用者，內外每個提供服務。 如果只有數區域中的資料可讓您已 split\ brained 或兩個區域 （內外） 已委派給相同的父系網域，這會成為管理問題。

>[!NOTE]
> - DNS 部署會 split\ 蛋有兩個版本的單一區域，內部使用者在組織的企業網路，一種版本，一種版本的外部使用者 –，通常是在網際網路上的使用者。
> - 此主題[適用於使用 DNS 原則 Split-Brain DNS 部署](split-brain-DNS-deployment.md)解釋如何使用 DNS 原則和區域領域部署 split\ 蛋 DNS 系統單一的 Windows Server 2016 DNS 伺服器上。



##  <a name="example-split-brain-dns-in-active-directory"></a>在 Active Directory 中範例 Split\ 蛋 DNS

此範例中使用虛構公司，以 Contoso，維持在 www.career.contoso.com 更上層樓網站。

網站有兩個版本，一個用於內部使用者內部職位何處可使用。 在本機的 IP 位址 10.0.0.39 使用此內部網站。 

第二個版本的相同的網站，可在公用 IP 位址 65.55.39.10 公用版本。

DNS 原則不存在，系統管理員，才能主機上不同的 Windows Server DNS 伺服器下列兩個區域和管理另行購買。 

使用 DNS 原則這些區域可以立即裝載相同的 DNS 伺服器上。

如果 contoso.com 的 DNS 伺服器 Active Directory 整合，且在兩個網路介面聆聽，以 Contoso DNS 系統管理員可以依照達成 split\ 蛋部署本主題中的步驟操作。

系統管理員 DNS 設定的 DNS 伺服器介面使用下列的 IP 位址。

- 網際網路面對網路介面卡的外部查詢 208.84.0.53 公用 IP 位址設定。
- 設定內部面對網路介面卡是內部查詢 10.0.0.56 私人 IP 位址。

下圖描述此案例。

![Split-Brain 廣告整合 DNS 部署](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>在 Active Directory 中蛋 Split\ DNS DNS 原則的方式

時所需的 DNS 原則設定的 DNS 伺服器，每個名稱解析要求被評估 DNS 伺服器上的原則。

伺服器介面用於在此範例中為準則內外戶端來區分公司。

如果時，收到查詢伺服器介面比對任何原則，相關的區域範圍用來查詢回應。 

因此，在我們的範例，私人 IP (10.0.0.56) 上接收 www.career.contoso.com DNS 查詢收到 DNS 回應包含內部 IP 位址。和在公用網路介面收到 DNS 查詢接收 DNS 回應包含 （這是正常查詢解析度一樣） 的區域預設範圍的公用 IP 位址。  

只有預設區域領域支援動態 DNS \(DDNS\) 更新和清除的支援。 內部戶端的服務的區域預設範圍，因為 Contoso DNS 系統管理員可以繼續使用（動態 DNS 或靜態）更新中 contoso.com 記錄現有的機制。預設 non\ 區域領域 \（例如，外部中的範圍此 example\），DDNS 或清除支援不提供。

### <a name="high-availability-of-policies"></a>原則的可用性

DNS 原則不是 Active Directory 整合。 而不會複寫 DNS 原則相同的 Active Directory 整合式的區域裝載的其他 DNS 伺服器。 

DNS 原則會儲存在本機的 DNS 伺服器。 您可以輕鬆地匯出原則 DNS 伺服器到另一個使用以下的範例 Windows PowerShell 命令。

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

如需詳細資訊，下列 Windows PowerShell 參考主題。

- [Get-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/get-dnsserverqueryresolutionpolicy)
- [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/add-dnsserverqueryresolutionpolicy)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>如何設定在 Active Directory 中蛋 Split\ DNS DNS 原則

若要設定 DNS Split-Brain 使用 DNS 原則部署，您必須使用下列的各節，提供詳細的設定指示操作。

### <a name="add-the-active-directory-integrated-zone"></a>新增 Active Directory 整合式的區域

您可以使用下列命令範例 Active Directory 整合的 contoso.com 區域加入 DNS 伺服器。

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

如需詳細資訊，請查看[新增-DnsServerPrimaryZone](https://technet.microsoft.com/library/jj649876.aspx)。

### <a name="create-the-scopes-of-the-zone"></a>建立的區域的領域

您可以使用本節分割 contoso.com 建立外部區域領域的區域。

時區領域是區域的唯一執行個體。 DNS 區域可以有多個區域領域，與每個包含 DNS 記錄它自己設定的區域範圍。 相同記錄可能會出現在多個領域，以不同的 IP 位址或相同的 IP 位址。 

在 Active Directory 整合區中新增這個新的時區範圍，因為區域範圍，以及在其中的記錄會複寫 Active Directory 透過到其他複本伺服器網域中。

根據預設，區域領域存在每個 DNS 區域。 這個區域領域作為區域，具有相同的名稱，並在這個領域中工作舊版 DNS 作業。 這個預設區域領域裝載內部 www.career.contoso.com 的版本。

您可以使用下列命令範例建立區域領域 DNS 伺服器上。

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

如需詳細資訊，請查看[新增-DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)。

### <a name="add-records-to-the-zone-scopes"></a>若要的區域領域加入資料

下一個步驟是新增兩代表網頁伺服器主機記錄區域領域外部及 [預設 \(for internal clients\)。 

在內部區域預設範圍，記錄 www.career.contoso.com 新增的 IP 位址 10.0.0.39，也就是私人 IP 位址。並在外部區域範圍，相同使用碼表進行 \(www.career.contoso.com\) 加入公用 IP 位址 65.55.39.10。 

記錄 \（兩者皆內部區域預設範圍和外部區域 scope\）將會自動複製上使用他們區域各個領域的網域。

若要將 DNS 伺服器上的時區領域加入資料來，您可以使用下列命令範例。

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

>[!NOTE]
>**– ZoneScope**當記錄新增到區域預設範圍時，不包含的參數。 這個動作會相同記錄加入一般的區域。

如需詳細資訊，請查看[新增-DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx)。

### <a name="create-the-dns-policies"></a>建立 DNS 原則

伺服器介面外部網路和連絡找出您所建立的時區範圍之後，您必須建立連接內外區域領域 DNS 原則。

>[!NOTE]
>此範例中使用伺服器介面 \（中範例命令 below\-ServerInterface 參數）來區分內外戶端條件為。 區分外部和內部另一個方法是使用 client 子網路為條件。 如果您找出內部戶端所屬的子網路，您可以設定來區分根據 client 子網路的 DNS 原則。 如何設定流量管理使用 client 子網路條件資訊，請查看[使用 DNS 原則主要伺服器的地理位置型流量管理的](primary-geo-location.md)。

當上公用 DNS 查詢接收設定原則之後，從區域的外部範圍傳回解答。 

>[!NOTE]
>不原則所需的對應內部區域預設範圍。 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

>[!NOTE]
>208.84.0.53 是在公用網路介面的 IP 位址。

如需詳細資訊，請查看[新增-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。

現在 DNS 伺服器設定所需的 DNS 原則的名稱 split-brain 伺服器 Active Directory 整合 DNS 的區域。

您可以建立數千 DNS 原則根據您的資料傳輸管理的需求，且所有的新原則已經套用動態-不需要重新 DNS 伺服器-連入查詢。 
