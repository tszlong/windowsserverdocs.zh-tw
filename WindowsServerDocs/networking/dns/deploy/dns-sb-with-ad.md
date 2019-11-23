---
title: 在 Active Directory 中使用適合拆分式 DNS 部署的 DNS 原則
description: 您可以使用本主題，利用 Windows Server 2016 中 Active Directory 整合式 DNS 區域，在分裂部署時運用 DNS 原則的流量管理功能。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1a05bdcbf6205b8be7044c92e3dcf71a6e62bed6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356033"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>在 Active Directory 中使用適合拆分式 DNS 部署的 DNS 原則

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題，利用 Windows Server 2016 中 Active Directory 整合的 DNS 區域來分割\-大腦部署的 DNS 原則的流量管理功能。

在 Windows Server 2016 中，DNS 原則支援已延伸到 Active Directory 整合的 DNS 區域。 Active Directory 整合為 DNS 伺服器提供多個\-主機高可用性功能。 

在過去，此案例需要 DNS 系統管理員維護兩部不同的 DNS 伺服器，每個都提供服務給每一組使用者（內部和外部）。 如果區域內只有少數記錄已分割\-brained，或區域的兩個實例（內部和外部）都已委派給相同的父系網域，這就成為了管理之間謎。

> [!NOTE]
> - 當單一區域有兩個版本時，會將 DNS 部署分割\-大腦，其中一個版本適用于組織內部網路上的內部使用者，另一個版本適用于外部使用者（通常是網際網路上的使用者）。
> - [使用適用于分裂式 Dns 部署的 Dns 原則](split-brain-DNS-deployment.md)主題說明如何使用 dns 原則和區域範圍，在單一 Windows SERVER 2016 dns 伺服器上部署分割\-大腦 dns 系統。



##  <a name="example-split-brain-dns-in-active-directory"></a>在 Active Directory 中分割\-大腦 DNS 的範例

這個範例會使用一個虛構的公司 Contoso，在 www.career.contoso.com 維護事業網站。

網站有兩個版本，一個用於內部作業張貼可用的內部使用者。 這個內部網站可在本機 IP 位址10.0.0.39 中取得。 

第二個版本是相同網站的公用版本，可在 [公用 IP 位址] 65.55.39.10 取得。

在沒有 DNS 原則的情況下，系統管理員必須在不同的 Windows Server DNS 伺服器上裝載這兩個區域，並分別加以管理。 

使用 DNS 原則，這些區域現在可以裝載在相同的 DNS 伺服器上。

如果 contoso.com 的 DNS 伺服器 Active Directory 整合，而且正在接聽兩個網路介面，Contoso DNS 系統管理員可以遵循本主題中的步驟，以達到分散的\-大腦部署。

DNS 系統管理員會使用下列 IP 位址來設定 DNS 伺服器介面。

- 網際網路對向網路介面卡已針對外部查詢，使用208.84.0.53 的公用 IP 位址進行設定。
- 內部網路對向網路介面卡的設定，其私人 IP 位址為10.0.0.56 以進行內部查詢。

下圖描述此案例。

![分割-大腦 AD 整合式 DNS 部署](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Active Directory 中分割\-大腦 DNS 的 DNS 原則如何運作

當 DNS 伺服器設定必要的 DNS 原則時，每個名稱解析要求都會根據 DNS 伺服器上的原則進行評估。

在此範例中，伺服器介面是用來區別內部和外部用戶端的準則。

如果接收查詢的伺服器介面符合任何原則，則會使用相關聯的區域範圍來回應查詢。 

因此，在我們的範例中，在私人 IP （10.0.0.56）上收到的 www.career.contoso.com DNS 查詢會收到包含內部 IP 位址的 DNS 回應。而且在公用網路介面上接收到的 DNS 查詢，會收到包含預設區域範圍中公用 IP 位址的 DNS 回應（這與一般查詢解析相同）。  

只有預設區域範圍支援動態 DNS \(DDNS\) 更新和清除。 因為內部用戶端是由預設區域範圍服務，所以 Contoso DNS 系統管理員可以繼續使用現有的機制（動態 DNS 或靜態）來更新 contoso.com 中的記錄。 若為非\-的預設區域範圍 \(例如此範例中的外部範圍\)，則無法使用 DDNS 或清除支援。

### <a name="high-availability-of-policies"></a>原則的高可用性

DNS 原則不 Active Directory 整合。 因此，DNS 原則不會複寫到裝載相同 Active Directory 整合區域的其他 DNS 伺服器。 

DNS 原則會儲存在本機 DNS 伺服器上。 您可以使用下列範例 Windows PowerShell 命令，輕鬆地將 DNS 原則從一部伺服器匯出到另一部伺服器。

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

如需詳細資訊，請參閱下列 Windows PowerShell 參考主題。

- [DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy?view=win10-ps)
- [新增-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>如何在 Active Directory 中設定分割\-大腦 DNS 的 DNS 原則

若要使用 DNS 原則設定 DNS 分割部署，您必須使用下列各節，其中提供詳細的設定指示。

### <a name="add-the-active-directory-integrated-zone"></a>新增 Active Directory 整合區域

您可以使用下列範例命令，將 Active Directory 整合式 contoso.com 區域新增到 DNS 伺服器。

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

如需詳細資訊，請參閱[add-dnsserverprimaryzone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverprimaryzone?view=win10-ps)。

### <a name="create-the-scopes-of-the-zone"></a>建立區域的範圍

您可以使用此區段來分割區域 contoso.com，以建立外部區域範圍。

區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，其中每個區域範圍都包含自己的一組 DNS 記錄。 相同的記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。 

因為您要在 Active Directory 整合區域中新增這個新區域範圍，所以區域範圍和其內部記錄會透過 Active Directory 複寫到網域中的其他複本伺服器。

根據預設，區域範圍會存在於每個 DNS 區域中。 此區域範圍與區域具有相同的名稱，且舊版 DNS 作業會在此範圍內運作。 此預設區域範圍將裝載 www.career.contoso.com 的內部版本。

您可以使用下列範例命令，在 DNS 伺服器上建立區域範圍。

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

如需詳細資訊，請參閱[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。

### <a name="add-records-to-the-zone-scopes"></a>將記錄新增至區域範圍

下一個步驟是將代表 web 伺服器主機的記錄新增至兩個區域範圍中-內部用戶端的外部和預設 \(\)。 

在預設的內部區域範圍中，會使用 IP 位址10.0.0.39 （也就是私人 IP 位址）新增記錄 www.career.contoso.com。而在外部區域範圍中，會使用公用 IP 位址65.55.39.10 來新增相同的記錄 \(www.career.contoso.com\)。 

預設內部區域範圍和外部區域\) 範圍中 \(的記錄都會自動複寫到網域及其各自的區域範圍。

您可以使用下列範例命令，將記錄新增至 DNS 伺服器上的區域範圍。

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

> [!NOTE]
> 當記錄新增至預設區域範圍時，不會包含 **– ZoneScope**參數。 此動作等同于將記錄新增至一般區域。

如需詳細資訊，請參閱[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

### <a name="create-the-dns-policies"></a>建立 DNS 原則

在您識別出外部網路和內部網路的伺服器介面，且已建立區域範圍之後，您必須建立連接內部和外部區域範圍的 DNS 原則。

> [!NOTE]
> 這個範例會使用伺服器介面 \(下列範例命令中的-ServerInterface 參數，\) 做為區別內部和外部用戶端的準則。 另一個要區別外部和內部用戶端的方法，是使用用戶端子網做為準則。 如果您可以識別內部用戶端所屬的子網，您可以設定 DNS 原則來根據用戶端子網區分。 如需如何使用用戶端子網準則設定流量管理的相關資訊，請參閱[將 DNS 原則用於以地理位置為基礎的流量管理與主伺服器](primary-geo-location.md)。

設定原則之後，在公用介面上收到 DNS 查詢時，就會從區域的外部範圍傳回答案。 

> [!NOTE]
> 對應預設的內部區域範圍不需要任何原則。 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

> [!NOTE]
> 208.84.0.53 是公用網路介面上的 IP 位址。

如需詳細資訊，請參閱[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

現在，DNS 伺服器已針對具有 Active Directory 整合式 DNS 區域的分割大腦名稱伺服器，使用必要的 DNS 原則進行設定。

您可以根據您的流量管理需求來建立數以千計的 DNS 原則，而所有的新原則都會以動態方式套用，而不需要重新開機 DNS 伺服器上的傳入查詢。 
