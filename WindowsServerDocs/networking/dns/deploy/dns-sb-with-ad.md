---
title: 在 Active Directory 中使用適合拆分式 DNS 部署的 DNS 原則
description: 您可以使用本主題來運用流量的裂腦的部署與 Active Directory 的 DNS 原則的管理功能整合的 Windows Server 2016 中的 DNS 區域。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7a5761cafff0a4bf148958a7f14aeaf311075b2e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839779"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>在 Active Directory 中使用適合拆分式 DNS 部署的 DNS 原則

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來運用流量管理功能的分割的 DNS 原則\-大腦部署與 Active Directory 整合的 Windows Server 2016 中的 DNS 區域。

在 Windows Server 2016 中，DNS 原則支援已擴充為 Active Directory 整合 DNS 區域。 Active Directory 整合提供多\-主要 DNS 伺服器的高可用性功能。 

先前，這種情節需要 DNS 系統管理員會在兩個不同的 DNS 伺服器，每個提供服務給每個使用者，內部和外部維護。 如果只有少數的記錄，在該區域內已分割\-是或已委派的區域 （內部和外部） 的兩個執行個體相同的父系網域中，這也成為管理難題。

>[!NOTE]
> - 分割 DNS 部署\-核心分裂時有單一區域的兩個版本，供內部使用者在組織內部的一個版本，針對外部使用者 – 是，一般而言，使用者在網際網路上的一個版本。
> - 本主題[使用 Split-Brain DNS 部署的 DNS 原則](split-brain-DNS-deployment.md)說明如何使用 DNS 原則和區域範圍部署分割\-大腦單一的 Windows Server 2016 的 DNS 伺服器上的 DNS 系統。



##  <a name="example-split-brain-dns-in-active-directory"></a>範例分割\-大腦 Active Directory 中的 DNS

此範例會使用一個虛構的公司 Contoso，維護 www.career.contoso.com 職涯網站。

站台會有兩個版本，一個用於內部 job 可用位置的內部使用者。 此內部的站台會位於本機的 IP 位址 10.0.0.39。 

第二個版本是相同的站台，可在公用 IP 位址 65.55.39.10 公用版本。

如果沒有使用 DNS 原則，系統管理員才可裝載這些不同的 Windows Server DNS 伺服器上的兩個區域，並個別加以管理。 

使用 DNS 原則這些區域可以現在被裝載於相同的 DNS 伺服器。

如果 contoso.com 的 DNS 伺服器是 Active Directory 整合，而且兩個網路介面上接聽，Contoso DNS 系統管理員可以依照本主題，以達到分割\-大腦部署。

DNS 系統管理員會使用下列的 IP 位址設定的 DNS 伺服器介面。

- 設定網際網路對向網路介面卡是 208.84.0.53 外部查詢的公用 IP 位址。
- 內部網路對向網路介面卡已使用的內部查詢 10.0.0.56 的私人 IP 位址。

下圖說明此案例。

![裂腦的 AD 整合式 DNS 部署](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>DNS 原則的分割方式\-腦中 Active Directory 的運作方式的 DNS

當 DNS 伺服器設定的所需的 DNS 原則時，會將每個名稱解析要求評估的 DNS 伺服器上的原則。

伺服器介面是在此範例中，做為準則用來區分內部和外部的用戶端。

如果查詢已接收的伺服器介面可符合任何原則，相關聯的區域範圍用來回應查詢。 

因此，在本例中，私人 IP (10.0.0.56) 所接收的 DNS 查詢的 www.career.contoso.com 收到的 DNS 回應，其中包含內部 IP 位址;和公用網路介面所接收的 DNS 查詢會收到包含公用 IP 位址 （這是一般的查詢解析相同） 的預設區域範圍中的 DNS 回應。  

支援動態 DNS \(DDNS\)更新及清除功能僅適用於預設區域範圍。 因為內部的用戶端由預設的區域範圍，Contoso DNS 系統管理員可以繼續使用現有的機制 （動態 DNS 或靜態） 來更新 contoso.com 中的記錄。 針對非\-預設區域範圍\(例如在此範例中的外部範圍\)，不提供 DDNS 或清除的支援。

### <a name="high-availability-of-policies"></a>高可用性的原則

DNS 原則不是 Active Directory 整合。 因為這個緣故，DNS 原則不會複寫到其他 DNS 伺服器裝載相同的 Active Directory 整合的區域。 

DNS 原則會儲存在本機的 DNS 伺服器上。 您可以輕鬆地匯出 DNS 原則從一部伺服器之間使用下列的範例 Windows PowerShell 命令。

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

如需詳細資訊，請參閱下列 Windows PowerShell 參考主題。

- [Get-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy?view=win10-ps)
- [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>如何設定分割 DNS 原則\-大腦 Active Directory 中的 DNS

若要使用 DNS 原則中設定 DNS Split-Brain 部署，您必須使用下列各節，提供詳細的設定指示。

### <a name="add-the-active-directory-integrated-zone"></a>新增 Active Directory 整合的區域

您可以使用下列範例命令將 Active Directory 整合式的 contoso.com 區域新增至 DNS 伺服器。

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

如需詳細資訊，請參閱 < [Add-dnsserverprimaryzone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverprimaryzone?view=win10-ps)。

### <a name="create-the-scopes-of-the-zone"></a>建立區域的範圍

您可以使用本節來分割區域 contoso.com，若要建立外部區域的範圍。

區域範圍內是區域的唯一的執行個體。 DNS 區域可以有多個區域範圍，與包含它自己的 DNS 記錄集的每個區域範圍。 同一筆記錄中可以存在多個領域，具有不同 IP 位址或相同的 IP 位址。 

因為您要新增此新的區域範圍，在 Active Directory 整合區域中，區域範圍和其內的記錄將會透過 Active Directory 複寫到其他網域中的複本伺服器。

根據預設，在區域範圍會存在於每個 DNS 區域中。 此區域範圍作為區域，具有相同的名稱，此範圍上運作的舊版 DNS 作業。 此預設區域範圍將會裝載 www.career.contoso.com 的內部版本。

您可以使用下列範例命令建立 DNS 伺服器上的區域範圍。

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。

### <a name="add-records-to-the-zone-scopes"></a>將記錄新增至區域範圍

下一個步驟是新增這兩個代表 web 伺服器主機的記錄區域範圍外部，預設值\(內部的用戶端\)。 

在預設的內部區域範圍內，記錄 www.career.contoso.com 新增 IP 位址 10.0.0.39，也就是私人的 IP 位址;和在外部的區域範圍內，相同的資料錄\(www.career.contoso.com\)加上的公用 IP 位址 65.55.39.10。 

資料錄\(內部預設值在區域範圍和外部的區域範圍\)會自動複寫到整個網域使用個別的區域範圍。

您可以使用下列範例命令將記錄新增至 DNS 伺服器上的區域範圍。

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

>[!NOTE]
>**– ZoneScope**記錄新增至預設區域範圍時，未包含參數。 此動作，等於是將記錄新增至正常的區域。

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

### <a name="create-the-dns-policies"></a>建立 DNS 原則

您已識別的外部網路和內部網路的伺服器介面，而且您已建立的區域範圍之後，您必須建立連線的內部和外部的區域範圍的 DNS 原則。

>[!NOTE]
>這個範例會使用伺服器介面\(-ServerInterface 參數，在下列範例命令\)為區分內部和外部的用戶端之間的篩選條件。 若要區分外部和內部的用戶端的另一種方法是使用用戶端子網路做為準則。 如果您可以識別內部的用戶端所屬的子網路，您可以設定來區分根據用戶端子網路的 DNS 原則。 如需如何設定使用用戶端的子網路條件的流量管理的詳細資訊，請參閱[地理位置與主要伺服器，根據流量管理的使用 DNS 原則](primary-geo-location.md)。

公用介面上收到 DNS 查詢時，設定原則之後，回應會傳回來自外部範圍的區域。 

>[!NOTE]
>沒有任何原則所需的對應預設內部區域範圍。 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

>[!NOTE]
>208.84.0.53 是公用網路介面上的 IP 位址。

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

現在的 DNS 伺服器已使用必要的 DNS 原則裂腦的名稱伺服器與 Active Directory 整合 DNS 區域。

您可以建立數以千計的 DNS 原則根據您的流量管理需求，而且所有新的原則都會套用動態-不需要重新啟動 DNS 伺服器-在傳入的查詢。 
