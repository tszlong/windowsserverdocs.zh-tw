---
title: 使用 DNS 原則以時間為基礎進行智慧型 DNS 回應
description: 本主題是 Windows Server 2016 DNS 原則案例指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e4bb075368bb3dfadaa8046b177dbbac637763e3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317828"
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>使用 DNS 原則以時間為基礎進行智慧型 DNS 回應

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解如何使用以當天時間為基礎的 DNS 原則，將應用程式流量分散到應用程式的不同地理位置。  
  
當您想要將一個時區的流量導向另一個時區的其他應用程式伺服器（例如 Web 服務器）時，此案例很有用。 這可讓您在主伺服器以流量超載時的尖峰時段，對應用程式實例之間的流量進行負載平衡。   
  
### <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day"></a><a name="bkmk_example1"></a>以當日時間為基礎的智慧型 DNS 回應範例  
以下範例示範如何使用 DNS 原則，根據一天的時間來平衡應用程式流量。  
  
這個範例使用一個虛構的公司 Contoso 禮物服務，透過其網站 contosogiftservices.com 提供全球各地的線上贈與解決方案。   
  
Contosogiftservices.com 網站裝載于兩個資料中心，一個位於西雅圖（北美洲），另一個在都柏林（歐洲）。 DNS 伺服器已設定為使用 DNS 原則傳送地理位置感知回應。 在最新的企業營運中，contosogiftservices.com 每天會有更多的訪客，而且有些客戶回報了服務可用性問題。  
  
Contoso 禮物服務會執行網站分析，併發現在當地時間下午6點和 9 PM 之間，Web 服務器的流量有激增。 Web 服務器無法調整以在這些尖峰時間處理增加的流量，因而導致客戶拒絕服務。 歐洲和美式資料中心都有相同的尖峰時數流量超載。 在每天的其他時間，伺服器會處理的流量量，會低於其最大容量。  
  
為了確保 contosogiftservices.com 客戶從網站獲得回應式體驗，Contoso 禮物服務想要將一些都柏林流量重新導向至都柏林的下午6點到 9 PM 之間的西雅圖應用程式伺服器;而且他們想要將一些西雅圖流量重新導向到西雅圖的下午6點到9點之間的都柏林應用程式伺服器。  
  
下圖描述此案例。  
  
![一天中的時間 DNS 原則範例](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="how-intelligent-dns-responses-based-on-time-of-day-works"></a><a name="bkmk_works1"></a>以每天時間為基礎的智慧型 DNS 回應的運作方式  
  
當 DNS 伺服器設定為一天的 DNS 原則時，在每個地理位置的下午6點到下午9點，DNS 伺服器會執行下列動作。  
  
- 以本機資料中心內 Web 服務器的 IP 位址，回答它所收到的前四個查詢。  
- 以遠端資料中心內 Web 服務器的 IP 位址，回答它所收到的第五個查詢。   
  
這個以原則為基礎的行為會將本機網頁伺服器流量負載的20個減少到遠端 Web 服務器，減輕對本機應用程式伺服器的負擔，並提升客戶的網站效能。  
  
在離峰時段，DNS 伺服器會執行以地理位置為基礎的標準流量管理。 此外，從北美洲或歐洲以外的位置傳送查詢的 DNS 用戶端，DNS 伺服器負載會平衡西雅圖與都柏林資料中心之間的流量。  
  
在 DNS 中設定多個 DNS 原則時，它們是一組已排序的規則，並由 DNS 從最高優先順序處理到最低優先順序。 DNS 會使用符合情況的第一個原則，包括一天中的時間。 基於這個理由，更具體的原則應具有較高的優先順序。 如果您建立一天的原則，並在原則清單中提供高優先順序，則 DNS 會先處理並使用這些原則（如果它們符合 DNS 用戶端查詢的參數和原則中所定義的準則）。 如果兩者不相符，DNS 會向下移動原則清單來處理預設原則，直到找到相符的原則為止。  
  
如需原則類型和準則的詳細資訊，請參閱[DNS 原則總覽](../../dns/deploy/DNS-Policies-Overview.md)。  
  
### <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day"></a><a name="bkmk_how1"></a>如何根據一天中的時間，設定智慧型 DNS 回應的 DNS 原則  
若要針對每日時間以應用程式負載平衡為基礎的查詢回應設定 DNS 原則，您必須執行下列步驟。  
  
- [建立 DNS 用戶端子網](#bkmk_subnets)  
- [建立區域範圍](#bkmk_zscopes)  
- [將記錄新增至區域範圍](#bkmk_records)  
- [建立 DNS 原則](#bkmk_policies)  
  
>[!NOTE]
>您必須在您想要設定之區域的授權 DNS 伺服器上執行這些步驟。 若要執行下列程式，必須要有**DnsAdmins**的成員資格或同等許可權。  
  
下列各節提供詳細的設定指示。  
  
>[!IMPORTANT]
>下列各節包含範例 Windows PowerShell 命令，其中包含許多參數的範例值。 執行這些命令之前，請務必將這些命令中的範例值取代為適用于您的部署的值。  
  
#### <a name="create-the-dns-client-subnets"></a><a name="bkmk_subnets"></a>建立 DNS 用戶端子網  
第一個步驟是識別您想要將流量重新導向之區域的子網或 IP 位址空間。 例如，如果您想要重新導向「美國」和「歐洲」的流量，則需要識別這些區域的子網或 IP 位址空間。  
  
您可以從地理 IP 對應取得此資訊。 根據這些地理 IP 散發套件，您必須建立「DNS 用戶端子網」。 DNS 用戶端子網是 IPv4 或 IPv6 子網的邏輯群組，會將查詢傳送到 DNS 伺服器。  
  
您可以使用下列 Windows PowerShell 命令來建立 DNS 用戶端子網。  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
如需詳細資訊，請參閱[DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
#### <a name="create-the-zone-scopes"></a><a name="bkmk_zscopes"></a>建立區域範圍  
設定用戶端子網之後，您必須將想要重新導向其流量的區域分割成兩個不同的區域範圍，也就是您已設定的每個 DNS 用戶端子網都有一個範圍。  
  
例如，如果您想要重新導向 DNS 名稱 www.contosogiftservices.com 的流量，您必須在 contosogiftservices.com 區域中建立兩個不同的區域範圍，一個用於美國，一個用於歐洲。  
  
區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，其中每個區域範圍都包含自己的一組 DNS 記錄。 相同的記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。  
  
>[!NOTE]
>根據預設，區域範圍會存在於 DNS 區域中。 此區域範圍與區域具有相同的名稱，且舊版 DNS 作業會在此範圍內運作。  
  
您可以使用下列 Windows PowerShell 命令來建立區域範圍。  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
如需詳細資訊，請參閱[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
#### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records"></a>將記錄新增至區域範圍  
現在您必須將代表 web 伺服器主機的記錄新增到這兩個區域範圍中。  
  
例如，在**SeattleZoneScope**中，會使用位於西雅圖資料中心的 IP 位址192.0.0.1 新增記錄<strong>www.contosogiftservices.com</strong> 。 同樣地，在**DublinZoneScope**中，記錄<strong>www.contosogiftservices.com</strong>會新增至都柏林資料中心內的 IP 位址141.1.0。3  
  
您可以使用下列 Windows PowerShell 命令，將記錄新增至區域範圍。  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
當您在預設範圍中新增記錄時，不會包含 ZoneScope 參數。 這與將記錄新增至標準 DNS 區域相同。  
  
如需詳細資訊，請參閱[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
#### <a name="create-the-dns-policies"></a><a name="bkmk_policies"></a>建立 DNS 原則  
建立子網、分割區（區域範圍）並新增記錄之後，您必須建立用來連接子網和分割區的原則，如此一來，當查詢來自其中一個 DNS 用戶端子網中的來源時，就會從傳回查詢回應區域的正確範圍。 對應預設區域範圍不需要任何原則。  
  
設定這些 DNS 原則之後，DNS 伺服器的行為如下所示：  
  
1. 歐洲的 DNS 用戶端會在其 DNS 查詢回應中，收到都柏林資料中心內 Web 服務器的 IP 位址。  
2. 美國的 DNS 用戶端會在其 DNS 查詢回應的西雅圖資料中心內，接收 Web 服務器的 IP 位址。  
3. 在都柏林的下午6點到下午9點，歐洲用戶端的20% 查詢會在其 DNS 查詢回應的西雅圖資料中心內收到 Web 服務器的 IP 位址。  
4. 在西雅圖下午6點到下午9點，美國用戶端的20% 查詢會在其 DNS 查詢回應中收到都柏林資料中心內 Web 服務器的 IP 位址。  
5. 來自世界各地的一半查詢會收到西雅圖資料中心的 IP 位址，而另一半則會收到都柏林資料中心的 IP 位址。  
  
  
您可以使用下列 Windows PowerShell 命令來建立連結 DNS 用戶端子網和區域範圍的 DNS 原則。  
  
>[!NOTE]
>在此範例中，DNS 伺服器是在 GMT 時區內，因此尖峰時數時間週期必須以相等的 GMT 時程表示。  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
如需詳細資訊，請參閱[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
現在，DNS 伺服器已設定必要的 DNS 原則，以根據地理位置和一天的時間重新導向流量。  
  
當 DNS 伺服器收到名稱解析查詢時，DNS 伺服器會根據所設定的 DNS 原則來評估 DNS 要求中的欄位。 如果名稱解析要求中的來源 IP 位址符合任何原則，則會使用相關聯的區域範圍來回應查詢，而使用者會被導向到地理位置最接近的資源。  
  
您可以根據您的流量管理需求來建立數以千計的 DNS 原則，而所有的新原則都會以動態方式套用，而不需要重新開機 DNS 伺服器上的傳入查詢。


