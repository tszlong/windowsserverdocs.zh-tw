---
title: 在 Active Directory 中使用適合拆分式 DNS 部署的 DNS 原則
description: 您可以使用本主題來利用 Windows Server 2016 中 Active Directory 整合式 DNS 區域的網路流量管理功能，進行分裂式部署。
manager: brianlic
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 423debd6538e2de8de7e2919e18e7da39fe8c733
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865327"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>在 Active Directory 中使用適合拆分式 DNS 部署的 DNS 原則

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來利用 \- Windows Server 2016 中 Active Directory 整合式 dns 區域的分割大腦部署的流量管理功能。

在 Windows Server 2016 中，DNS 原則支援延伸至 Active Directory 整合式 DNS 區域。 Active Directory 整合 \- 為 DNS 伺服器提供多宿主高可用性功能。

先前，在此案例中，DNS 系統管理員需要維護兩部不同的 DNS 伺服器，每一部都為內部和外部的每一組使用者提供服務。 如果區域內只有少數記錄被分割 \- brained 或區域的兩個實例 (內部和外部) 委派給相同的父系網域，則這會成為管理之間謎。

> [!NOTE]
> - \-當有兩個版本的單一區域、一個適用于組織內部網路上的內部使用者，以及一個適用于外部使用者的版本（通常是網際網路上的使用者）時，DNS 部署就會分裂。
> - [Split-Brain Dns 部署的 Dns 原則](split-brain-DNS-deployment.md)主題將說明如何使用 dns 原則和區域範圍， \- 在單一 WINDOWS Server 2016 dns 伺服器上部署分割的大腦 dns 系統。

## <a name="example-split-brain-dns-in-active-directory"></a>\-Active Directory 中分割大腦 DNS 的範例

這個範例會使用一個虛構公司 Contoso，在 www.career.contoso.com 維護一個事業網站。

網站有兩個版本，一個適用于內部工作張貼的內部使用者。 此內部網站可在本機 IP 位址10.0.0.39 取得。

第二個版本是相同網站的公用版本，可在公用 IP 位址65.55.39.10 取得。

如果沒有 DNS 原則，系統管理員必須在個別的 Windows Server DNS 伺服器上裝載這兩個區域，並分開管理它們。

使用 DNS 原則時，這些區域現在可以裝載在相同的 DNS 伺服器上。

如果 contoso.com 的 DNS 伺服器 Active Directory 整合式，並在兩個網路介面上接聽，Contoso DNS 系統管理員可以依照本主題中的步驟進行，以達成分裂的 \- 大腦部署。

DNS 系統管理員會使用下列 IP 位址來設定 DNS 伺服器介面。

- 網際網路面向的網路介面卡設定為使用208.84.0.53 的公用 IP 位址來進行外部查詢。
- 內部網路對應的網路介面卡設定為內部查詢的私人 IP 位址10.0.0.56。

下圖描述此案例。

![Split-Brain AD 整合式 DNS 部署](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Active Directory 中分割大腦 DNS 的 DNS 原則如何 \- 運作

使用必要的 DNS 原則設定 DNS 伺服器時，會針對 DNS 伺服器上的原則評估每個名稱解析要求。

在此範例中，伺服器介面是用來區分內部和外部用戶端的準則。

如果收到查詢的伺服器介面符合任何原則，則會使用相關聯的區域範圍來回應查詢。

因此，在我們的範例中，在私人 IP () 10.0.0.56 收到的 www.career.contoso.com DNS 查詢會收到包含內部 IP 位址的 DNS 回應;而在公用網路介面上接收的 DNS 查詢會收到包含預設區域範圍中公用 IP 位址的 DNS 回應 (這與一般查詢解析) 相同。

\( \) 只有預設區域範圍才支援動態 DNS DDNS 更新和清除。 因為內部用戶端是由預設區域範圍服務，所以 Contoso DNS 系統管理員可以繼續使用現有的機制 (動態 DNS 或靜態) 來更新 contoso.com 中的記錄。 針對非 \- 預設區域範圍 \( （例如此範例中的外部範圍 \) ），無法使用 DDNS 或清除支援。

### <a name="high-availability-of-policies"></a>原則的高可用性

DNS 原則未 Active Directory 整合。 因此，DNS 原則不會複寫到其他裝載相同 Active Directory 整合式區域的 DNS 伺服器。

DNS 原則會儲存在本機 DNS 伺服器上。 您可以使用下列範例 Windows PowerShell 命令，輕鬆地將 DNS 原則從一部伺服器匯出至另一部伺服器。

```powershell
$policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
$policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02
```

如需詳細資訊，請參閱下列 Windows PowerShell 參考主題。

- [DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy)
- [新增-DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy)

## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>如何 \- 在 Active Directory 中設定分割大腦 dns 的 Dns 原則

若要使用 DNS 原則設定 DNS Split-Brain 部署，您必須使用下列各節來提供詳細的設定指示。

### <a name="add-the-active-directory-integrated-zone"></a>新增 Active Directory 整合區域

您可以使用下列範例命令，將 Active Directory 整合 contoso.com 區域新增至 DNS 伺服器。

```powershell
Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru
```

如需詳細資訊，請參閱 [add-dnsserverprimaryzone](/powershell/module/dnsserver/add-dnsserverprimaryzone)。

### <a name="create-the-scopes-of-the-zone"></a>建立區域的範圍

您可以使用此區段來分割區域 contoso.com，以建立外部區域範圍。

區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，每個區域範圍都包含一組專屬的 DNS 記錄。 相同記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。

因為您要在 Active Directory 整合式區域中新增這個新的區域範圍，所以區域範圍和其中的記錄會透過 Active Directory 複寫到網域中的其他複本伺服器。

依預設，每個 DNS 區域中都有區域範圍。 此區域範圍與區域具有相同的名稱，且舊版 DNS 作業可在此範圍內運作。 此預設區域範圍將裝載 www.career.contoso.com 的內部版本。

您可以使用下列範例命令，在 DNS 伺服器上建立區域範圍。

```powershell
Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"
```

如需詳細資訊，請參閱 [DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope)。

### <a name="add-records-to-the-zone-scopes"></a>將記錄新增至區域範圍

下一步是將代表 web 伺服器主機的記錄新增至兩個區域範圍-外部和預設 \( 的內部用戶端 \) 。

在預設的內部區域範圍中，會使用 IP 位址10.0.0.39 （也就是私人 IP 位址）來新增 [記錄] www.career.contoso.com。此外，在外部區域範圍中， \( \) 會使用公用 IP 位址65.55.39.10 來新增相同的記錄 www.career.contoso.com。

\(預設內部區域範圍和外部區域範圍中的記錄 \) 都會自動以其各自的區域範圍複寫到整個網域。

您可以使用下列範例命令，將記錄新增至 DNS 伺服器上的區域範圍。

```powershell
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”
```

> [!NOTE]
> 將記錄新增至預設區域範圍時，不會包含 **– ZoneScope** 參數。 這個動作與將記錄新增至一般區域相同。

如需詳細資訊，請參閱 [DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord)。

### <a name="create-the-dns-policies"></a>建立 DNS 原則

在您識別出外部網路和內部網路的伺服器介面，並且已建立區域範圍之後，您必須建立 DNS 原則來連接內部和外部區域範圍。

> [!NOTE]
> 此範例會使用 \( 下列範例命令中的伺服器介面 ServerInterface 參數， \) 做為區分內部和外部用戶端的準則。 另一種區分外部和內部用戶端的方法是使用用戶端子網作為準則。 如果您可以識別內部用戶端所屬的子網，您可以設定 DNS 原則以根據用戶端子網區分。 如需有關如何使用用戶端子網準則設定流量管理的詳細資訊，請參閱 [使用 DNS 原則進行以 Geo-Location 為基礎的流量管理與主伺服器](primary-geo-location.md)。

設定原則之後，在公用介面上收到 DNS 查詢時，會從區域的外部範圍傳回答案。

> [!NOTE]
> 對應預設內部區域範圍不需要任何原則。

```powershell
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com
```

> [!NOTE]
> 208.84.0.53 是公用網路介面上的 IP 位址。

如需詳細資訊，請參閱 [DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy)。

現在 DNS 伺服器會使用具有 Active Directory 整合式 DNS 區域的分割區名稱伺服器所需的 DNS 原則進行設定。

您可以根據您的流量管理需求建立數千個 DNS 原則，而且會動態套用所有新原則，而不需要重新開機 DNS 伺服器上的傳入查詢。
