---
title: 使用適合拆分式 DNS 部署的 DNS 原則
description: 本主題是 Windows Server 2016 DNS 原則案例指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f0274eddba5aa81a0910ca2f22841029c699bb3e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962302"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>使用適用于分割 \- 大腦 Dns 部署的 Dns 原則

>適用於：Windows Server 2016

您可以使用本主題來瞭解如何在 Windows Server 2016 中設定 &reg; 分割區 dns 部署的 dns 原則，其中有兩個版本的單一區域-一個用於組織內部網路上的內部使用者，另一個用於外部使用者（通常是網際網路上的使用者）。

>[!NOTE]
>如需有關如何將 DNS 原則用於使用 \- Active Directory 整合式 Dns 區域的分割大腦 dns 部署的詳細資訊，請參閱[在 Active Directory 中將 dns 原則用於分割大腦 dns](dns-sb-with-ad.md)。

在過去，此案例需要 DNS 系統管理員維護兩部不同的 DNS 伺服器，每個都提供服務給每一組使用者（內部和外部）。 如果區域內只有少數記錄是分割的 \- brained，或區域的兩個實例 (內部和外部) 已委派給相同的父系網域，這就成為了管理之間謎。

分割大腦部署的另一個設定案例是 DNS 名稱解析的選擇性遞迴控制。 在某些情況下，企業 DNS 伺服器預期會透過網際網路對內部使用者執行遞迴解析，而它們也必須作為外部使用者的授權名稱伺服器，並封鎖它們的遞迴。

本主題包含下列各節。

- [DNS 分割大腦部署的範例](#bkmk_sbexample)
- [DNS 選擇性遞迴控制的範例](#bkmk_recursion)

## <a name="example-of-dns-split-brain-deployment"></a><a name="bkmk_sbexample"></a>DNS 分割大腦部署的範例
以下是如何使用 DNS 原則來完成先前所述之分裂式 DNS 案例的範例。

此章節包含下列主題。

- [DNS 分割大腦部署的運作方式](#bkmk_sbhow)
- [如何設定 DNS 分割大腦部署](#bkmk_sbconfigure)

這個範例會使用一個虛構的公司 Contoso，在 www.career.contoso.com 維護事業網站。

網站有兩個版本，一個用於內部作業張貼可用的內部使用者。 這個內部網站可在本機 IP 位址10.0.0.39 中取得。

第二個版本是相同網站的公用版本，可在 [公用 IP 位址] 65.55.39.10 取得。

在沒有 DNS 原則的情況下，系統管理員必須在不同的 Windows Server DNS 伺服器上裝載這兩個區域，並分別加以管理。

使用 DNS 原則，這些區域現在可以裝載在相同的 DNS 伺服器上。

下圖描述此案例。

![分裂式 DNS 部署](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)

## <a name="how-dns-split-brain-deployment-works"></a><a name="bkmk_sbhow"></a>DNS 分割大腦部署的運作方式

當 DNS 伺服器設定必要的 DNS 原則時，每個名稱解析要求都會根據 DNS 伺服器上的原則進行評估。

在此範例中，伺服器介面是用來區別內部和外部用戶端的準則。

如果接收查詢的伺服器介面符合任何原則，則會使用相關聯的區域範圍來回應查詢。

因此，在我們的範例中，在私人 IP 上收到的 www.career.contoso.com DNS 查詢 (10.0.0.56) 會收到包含內部 IP 位址的 DNS 回應;而在公用網路介面上接收到的 DNS 查詢，會收到包含預設區域範圍中公用 IP 位址的 DNS 回應 (這與一般查詢解析) 相同。

## <a name="how-to-configure-dns-split-brain-deployment"></a><a name="bkmk_sbconfigure"></a>如何設定 DNS 分割大腦部署
若要使用 DNS 原則設定 DNS 分割部署，您必須使用下列步驟。

- [建立區域範圍](#bkmk_zscopes)
- [將記錄新增至區域範圍](#bkmk_records)
- [建立 DNS 原則](#bkmk_policies)

下列各節提供詳細的設定指示。

>[!IMPORTANT]
>下列各節包含範例 Windows PowerShell 命令，其中包含許多參數的範例值。 執行這些命令之前，請務必將這些命令中的範例值取代為適用于您的部署的值。

### <a name="create-the-zone-scopes"></a><a name="bkmk_zscopes"></a>建立區域範圍

區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，其中每個區域範圍都包含自己的一組 DNS 記錄。 相同的記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。

> [!NOTE]
> 根據預設，區域範圍會存在於 DNS 區域中。 此區域範圍與區域具有相同的名稱，且舊版 DNS 作業會在此範圍內運作。 此預設區域範圍將裝載 www.career.contoso.com 的外部版本。

您可以使用下列範例命令來分割區域範圍 contoso.com，以建立內部區域範圍。 內部區域範圍將用來保留 www.career.contoso.com 的內部版本。

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

如需詳細資訊，請參閱[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records"></a>將記錄新增至區域範圍

下一個步驟是將代表 Web 服務器主機的記錄新增至兩個區域範圍內：外部用戶端) 的內部和預設 (。

在內部區域範圍中，會使用 IP 位址10.0.0.39 （也就是私人 IP）新增記錄<strong>www.career.contoso.com</strong> 。而在預設區域範圍中，會使用 IP 位址65.55.39.10 加入相同的記錄<strong>www.career.contoso.com</strong>。

No **–** 當記錄新增至預設區域範圍時，下列範例命令中提供了 ZoneScope 參數。 這類似于將記錄新增至 vanilla 區域。

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

如需詳細資訊，請參閱[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

### <a name="create-the-dns-policies"></a><a name="bkmk_policies"></a>建立 DNS 原則

在您識別出外部網路和內部網路的伺服器介面，且已建立區域範圍之後，您必須建立連接內部和外部區域範圍的 DNS 原則。

>[!NOTE]
>這個範例會使用伺服器介面做為準則，以區別內部和外部用戶端。 另一個要區別外部和內部用戶端的方法，是使用用戶端子網做為準則。 如果您可以識別內部用戶端所屬的子網，您可以設定 DNS 原則來根據用戶端子網區分。 如需如何使用用戶端子網準則設定流量管理的相關資訊，請參閱[將 DNS 原則用於以地理位置為基礎的流量管理與主伺服器](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers)。

當 DNS 伺服器收到私用介面的查詢時，會從內部區域範圍傳回 DNS 查詢回應。

>[!NOTE]
>對應預設區域範圍不需要任何原則。

在下列範例命令中，10.0.0.56 是私用網路介面上的 IP 位址，如上圖所示。

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

如需詳細資訊，請參閱[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

## <a name="example-of-dns-selective-recursion-control"></a><a name="bkmk_recursion"></a>DNS 選擇性遞迴控制的範例

以下範例示範如何使用 DNS 原則來完成先前所述的 DNS 選擇性遞迴控制案例。

此章節包含下列主題。

- [DNS 選擇性遞迴控制的運作方式](#bkmk_recursionhow)
- [如何設定 DNS 選擇性遞迴控制](#bkmk_recursionconfigure)

這個範例使用與上一個範例 Contoso 相同的虛構公司，它會在 www.career.contoso.com 維護事業網站。

在 DNS 分割部署範例中，相同的 DNS 伺服器會同時回應外部和內部用戶端，並提供不同的答案。

某些 DNS 部署可能需要相同的 DNS 伺服器來執行內部用戶端的遞迴名稱解析，而不是作為外部用戶端的授權名稱伺服器。 這種情況稱為「DNS 選擇性遞迴控制」。

在舊版的 Windows Server 中，啟用遞迴表示它已在所有區域的整個 DNS 伺服器上啟用。 因為 DNS 伺服器也會接聽外部查詢，所以會為內部和外部用戶端啟用遞迴，讓 DNS 伺服器成為開啟的解析程式。

設定為開啟解析程式的 DNS 伺服器可能會很容易遭到資源耗盡，並可被惡意用戶端濫用來建立反映攻擊。

因此，Contoso DNS 系統管理員不想讓 contoso.com 的 DNS 伺服器針對外部用戶端執行遞迴名稱解析。 內部用戶端只需要遞迴控制，而可以針對外部用戶端封鎖遞迴控制。

下圖描述此案例。

![選擇性遞迴控制](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg)


### <a name="how-dns-selective-recursion-control-works"></a><a name="bkmk_recursionhow"></a>DNS 選擇性遞迴控制的運作方式

如果收到的查詢是未授權的 Contoso DNS 伺服器（例如 https://www.microsoft.com ），則會針對 DNS 伺服器上的原則來評估名稱解析要求。

因為這些查詢不會落在任何區域底下，所以 \( 不會評估分割大腦範例中所定義的區域層級原則 \) 。

DNS 伺服器會評估遞迴原則，而在私用介面上接收的查詢則符合**SplitBrainRecursionPolicy**。 此原則會指向已啟用遞迴的遞迴範圍。

然後，DNS 伺服器會執行遞迴以 https://www.microsoft.com 從網際網路取得答案，並在本機快取回應。

如果在外部介面上收到查詢，則不會符合任何 DNS 原則，而且會套用預設遞迴設定（在此案例中為**停用**）。

這可防止伺服器做為外部用戶端的開啟解析程式，同時做為內部用戶端的快取解析程式。

### <a name="how-to-configure-dns-selective-recursion-control"></a><a name="bkmk_recursionconfigure"></a>如何設定 DNS 選擇性遞迴控制

若要使用 DNS 原則設定 DNS 選擇性遞迴控制，您必須使用下列步驟。

- [建立 DNS 遞迴範圍](#bkmk_recscopes)
- [建立 DNS 遞迴原則](#bkmk_recpolicy)

#### <a name="create-dns-recursion-scopes"></a><a name="bkmk_recscopes"></a>建立 DNS 遞迴範圍

遞迴範圍是一組設定的唯一實例，可控制 DNS 伺服器上的遞迴。 遞迴範圍包含轉寄站的清單，並指定是否啟用遞迴。 DNS 伺服器可以有許多遞迴範圍。

舊版遞迴設定和轉寄站清單稱為「預設遞迴範圍」。 您無法新增或移除預設遞迴範圍（以名稱點 \( "." 識別） \) 。

在此範例中，預設遞迴設定是停用的，而內部用戶端的新遞迴範圍則是在啟用遞迴的情況下建立。

```powershell
Set-DnsServerRecursionScope -Name . -EnableRecursion $False
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True
```

如需詳細資訊，請參閱[DnsServerRecursionScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="create-dns-recursion-policies"></a><a name="bkmk_recpolicy"></a>建立 DNS 遞迴原則

您可以建立 DNS 伺服器遞迴原則，為一組符合特定準則的查詢選擇遞迴範圍。

如果 DNS 伺服器不是某些查詢的授權，則 DNS 伺服器遞迴原則可讓您控制如何解決查詢。

在此範例中，已啟用遞迴的內部遞迴範圍會與私人網路介面相關聯。

您可以使用下列範例命令來設定 DNS 遞迴原則。

```powershell
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
```

如需詳細資訊，請參閱[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

現在，DNS 伺服器已使用針對內部用戶端啟用選擇性遞迴控制的分割大腦名稱伺服器或 DNS 伺服器，來設定所需的 DNS 原則。

您可以根據您的流量管理需求來建立數以千計的 DNS 原則，而所有的新原則都會以動態方式套用，而不需要重新開機 DNS 伺服器上的傳入查詢。

如需詳細資訊，請參閱[DNS 原則案例指南](DNS-Policy-Scenario-Guide.md)。
