---
title: 使用適合拆分式 DNS 部署的 DNS 原則
description: 本主題是 DNS 原則案例指南的 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4ec4bc8e77e8411101b9a2b83a85ad5e1a0765b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873499"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>使用 DNS 原則進行分割\-大腦 DNS 部署

>適用於：Windows Server 2016

您可以使用本主題來了解如何設定 Windows Server 中的 DNS 原則&reg;2016 拆分式 DNS 部署，其中有兩個版本的單一區域-一個用於內部使用者，在您的組織內部網路上，一個用於外部使用者，使用者通常是在網際網路上的使用者。

>[!NOTE]
>如需如何使用 DNS 原則進行分割的詳細資訊\-大腦 DNS 部署與 Active Directory 整合 DNS 區域，請參閱 <<c2> [ 使用 Active Directory 中的 Split-Brain DNS 的 DNS 原則](dns-sb-with-ad.md)。

先前，這種情節需要 DNS 系統管理員會在兩個不同的 DNS 伺服器，每個提供服務給每個使用者，內部和外部維護。 如果只有少數的記錄，在該區域內已分割\-是或已委派的區域 （內部和外部） 的兩個執行個體相同的父系網域中，這也成為管理難題。 

拆分式部署的另一個組態案例是選擇性的遞迴控制項進行 DNS 名稱解析。 在某些情況下，企業的 DNS 伺服器應該執行遞迴解析內部的使用者，在網際網路上，同時也必須做為外部使用者的授權名稱伺服器，並封鎖遞迴。 

本主題涵蓋下列各節。

- [拆分式 DNS 部署的範例](#bkmk_sbexample)
- [DNS 選擇性的遞迴控制項範例](#bkmk_recursion)

##<a name="bkmk_sbexample"></a>拆分式 DNS 部署的範例
以下是如何使用 DNS 原則來完成先前所述的拆分式 DNS 案例的範例。

本節包含下列主題：

- [拆分式 DNS 部署的運作方式](#bkmk_sbhow)
- [如何設定的拆分式 DNS 部署](#bkmk_sbconfigure)


此範例會使用一個虛構的公司 Contoso，維護 www.career.contoso.com 職涯網站。

站台會有兩個版本，一個用於內部 job 可用位置的內部使用者。 此內部的站台會位於本機的 IP 位址 10.0.0.39。 

第二個版本是相同的站台，可在公用 IP 位址 65.55.39.10 公用版本。

如果沒有使用 DNS 原則，系統管理員才可裝載這些不同的 Windows Server DNS 伺服器上的兩個區域，並個別加以管理。 

使用 DNS 原則這些區域可以現在被裝載於相同的 DNS 伺服器。  

下圖說明此案例。

![拆分式 DNS 部署](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


##<a name="bkmk_sbhow"></a>拆分式 DNS 部署的運作方式

當 DNS 伺服器設定的所需的 DNS 原則時，會將每個名稱解析要求評估的 DNS 伺服器上的原則。

伺服器介面是在此範例中，做為準則用來區分內部和外部的用戶端。

如果查詢已接收的伺服器介面可符合任何原則，相關聯的區域範圍用來回應查詢。 

因此，在本例中，私人 IP (10.0.0.56) 所接收的 DNS 查詢的 www.career.contoso.com 收到的 DNS 回應，其中包含內部 IP 位址;和公用網路介面所接收的 DNS 查詢會收到包含公用 IP 位址 （這是一般的查詢解析相同） 的預設區域範圍中的 DNS 回應。  

##<a name="bkmk_sbconfigure"></a>如何設定的拆分式 DNS 部署
若要使用 DNS 原則中設定 DNS Split-Brain 部署，您必須使用下列步驟。

- [建立區域範圍](#bkmk_zscopes)  
- [將記錄新增至區域範圍](#bkmk_records)  
- [建立 DNS 原則](#bkmk_policies)

下列各節提供詳細的設定指示。

>[!IMPORTANT]
>下列各節包含 Windows PowerShell 命令範例包含許多參數的範例值。 請確定這些命令列中的範例值取代是適用於您的部署，然後再執行這些命令的值。 

###<a name="bkmk_zscopes"></a>建立區域範圍

區域範圍內是區域的唯一的執行個體。 DNS 區域可以有多個區域範圍，與包含它自己的 DNS 記錄集的每個區域範圍。 同一筆記錄中可以存在多個領域，具有不同 IP 位址或相同的 IP 位址。 

>[!NOTE]
>根據預設，在區域範圍存在於 DNS 區域。 此區域範圍作為區域，具有相同的名稱，此範圍上運作的舊版 DNS 作業。 此預設區域範圍將會裝載外部 www.career.contoso.com 的版本。

您可以使用下列範例命令，來分割區域範圍 contoso.com 建立內部區域範圍。 內部區域範圍，將用於保留 www.career.contoso.com 內部版本。

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

如需詳細資訊，請參閱[新增 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

###<a name="bkmk_records"></a>將記錄新增至區域範圍

下一個步驟是新增代表 Web 伺服器主機為兩個區域範圍-內部和預設 （適用於外部的用戶端） 的記錄。 

在內部區域範圍內，資料錄**www.career.contoso.com**會新增的 IP 位址 10.0.0.39，也就是私人的 ip 位址; 以及預設區域範圍中相同的記錄， **www.career.contoso.com**，是新增使用 65.55.39.10 的 IP 位址。

否 **– ZoneScope**記錄被新增到預設區域範圍時，將會提供在下列範例命令中的參數。 這是將記錄新增至 vanilla 區域類似。

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

###<a name="bkmk_policies"></a>建立 DNS 原則

您已識別的外部網路和內部網路的伺服器介面，而且您已建立的區域範圍之後，您必須建立連線的內部和外部的區域範圍的 DNS 原則。

>[!NOTE]
>若要區分內部和外部的用戶端，此範例會使用伺服器介面做為準則。 若要區分外部和內部的用戶端的另一種方法是使用用戶端子網路做為準則。 如果您可以識別內部的用戶端所屬的子網路，您可以設定來區分根據用戶端子網路的 DNS 原則。 如需如何設定使用用戶端的子網路條件的流量管理的詳細資訊，請參閱[地理位置與主要伺服器，根據流量管理的使用 DNS 原則](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers)。

當 DNS 伺服器收到的私用介面上的查詢時，DNS 查詢回應會傳回從內部區域範圍。

>[!NOTE]
>沒有任何原則所需的對應預設區域範圍。 

在下列範例命令中，10.0.0.56 都是私用網路介面上的 IP 位址，如上圖所示。

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  


## <a name="bkmk_recursion"></a>DNS 選擇性的遞迴控制項範例

以下是如何使用 DNS 原則來完成先前所述的案例中的 DNS 選擇性的遞迴控制項的範例。

本節包含下列主題：

- [如何 DNS 選擇性的遞迴控制項如何運作](#bkmk_recursionhow)
- [如何設定 DNS 選擇性的遞迴控制項](#bkmk_recursionconfigure)

此範例會使用相同的虛構公司，如上述範例中，Contoso，維護 www.career.contoso.com 職涯網站所示。

在 DNS 裂腦的部署範例中，相同的 DNS 伺服器會回應外部和內部用戶端，並提供不同的答案。 

某些 DNS 部署可能需要進行遞迴性名稱解析為內部的用戶端，除了作為外部用戶端授權名稱伺服器相同的 DNS 伺服器。 這種情況下就稱為 DNS 選擇性的遞迴功能控制項。

在舊版的 Windows Server 中，啟用遞迴的用途，已啟用的所有區域的整個 DNS 伺服器上。 因為 DNS 伺服器也會接聽外部查詢中，會啟用遞迴功能對於內部和外部用戶端，讓 DNS 伺服器開啟的解析程式。 

設定為開啟的解析程式可能容易受到資源耗盡，可能會遭到濫用建立反映攻擊的惡意用戶端的 DNS 伺服器。 

因此，Contoso DNS 系統管理員不想進行遞迴性名稱解析的外部用戶端 contoso.com 的 DNS 伺服器。 遞迴控制項可封鎖外部的用戶端時，就只需要遞迴控制項內部的用戶端。 

下圖說明此案例。

![選擇性的遞迴控制項](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>如何 DNS 選擇性的遞迴控制項如何運作

如果收到 Contoso DNS 伺服器的非權威的查詢時，例如 www.microsoft.com，然後名稱解析要求會評估針對 DNS 伺服器上的原則。 

因為這些查詢未落在任何區域，區域層級原則\(裂腦的範例中所定義\)不會評估。 

DNS 伺服器會先評估的遞迴原則，並接收私用介面的相符項目查詢**SplitBrainRecursionPolicy**。 此原則會指向遞迴範圍會啟用遞迴功能。

DNS 伺服器然後執行從網際網路，取得 www.microsoft.com 的答案的遞迴，並會快取在本機的回應。 

如果查詢上的外部介面、 沒有 DNS 原則相符項目和預設遞迴設定為在此情況下收到**已停用**-套用。

這可防止伺服器做為外部的用戶端，請開啟解析程式，而它做為內部用戶端的快取解析程式。 

### <a name="bkmk_recursionconfigure"></a>如何設定 DNS 選擇性的遞迴控制項

若要使用 DNS 原則中設定 DNS 選擇性的遞迴控制項，您必須使用下列步驟。

- [建立 DNS 遞迴範圍](#bkmk_recscopes)
- [建立 DNS 遞迴原則](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>建立 DNS 遞迴範圍

遞迴領域都是唯一的執行個體的一組控制遞迴 DNS 伺服器上的設定。 遞迴範圍包含的轉寄站清單，並指定是否會啟用遞迴功能。 DNS 伺服器可以有許多的遞迴範圍。 

轉寄站清單與傳統遞迴設定稱為預設遞迴範圍。 您無法新增或移除預設的遞迴領域，識別名稱點\("。"\).

在此範例中，已停用遞迴預設，會啟用遞迴功能來建立新的遞迴範圍，為內部用戶端時。

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

如需詳細資訊，請參閱[新增 DnsServerRecursionScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="bkmk_recpolicy"></a>建立 DNS 遞迴原則

您可以建立 DNS 伺服器遞迴原則選擇一組查詢符合特定準則的遞迴範圍。 

如果不授權某些查詢的 DNS 伺服器，DNS 伺服器遞迴原則可讓您控制如何解析查詢。 

在此範例中，遞迴已啟用內部遞迴範圍是私用網路介面相關聯。

您可以使用下列範例命令，設定 DNS 遞迴原則。

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
    

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

現在的 DNS 伺服器已裂腦的名稱伺服器或 DNS 伺服器所需的 DNS 原則設定為內部用戶端啟用的選擇性的遞迴控制。

您可以建立數以千計的 DNS 原則根據您的流量管理需求，而且所有新的原則都會套用動態-不需要重新啟動 DNS 伺服器-在傳入的查詢。 

如需詳細資訊，請參閱 < [DNS 原則案例指南](DNS-Policy-Scenario-Guide.md)。
