---
title: 使用 DNS 原則以時間為基礎進行智慧型 DNS 回應
description: 本主題是 DNS 原則案例指南的 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c36475dacb8664352f4ab270878357118d281c60
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446426"
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>使用 DNS 原則以時間為基礎進行智慧型 DNS 回應

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解如何將應用程式流量分散到不同地理位置分散的應用程式執行個體中，使用 DNS 原則為基礎的當日時間。  
  
這個案例是在您想要將流量導向至替代的應用程式伺服器，例如 Web 伺服器，另一個時區中的一個時區中的情況下很有用。 這可讓您將應用程式執行個體之間的平衡流量負載尖峰期間時間的週期，您的主要伺服器的流量多載。   
  
### <a name="bkmk_example1"></a>一天的時間為基礎的智慧型 DNS 回應的範例  
以下是如何使用 DNS 原則以一天的時間為基礎的應用程式流量平衡的範例。  
  
這個範例會使用一個虛構的公司 Contoso 禮物卡服務，可透過其網站在全球各地提供線上贈與解決方案，contosogiftservices.com。   
  
Contosogiftservices.com 網站裝載於兩個資料中心，一個西雅圖 （北美地區），另一個在都柏林 （歐洲）。 DNS 伺服器已設定傳送地理位置感知使用 DNS 原則的回應。 與商務最近激增，contosogiftservices.com 較高數目的訪客每一天，而且某些客戶已回報服務可用性問題。  
  
Contoso 禮物卡服務執行站台分析，並探索下午 6 點與下午 9 點的當地時間之間的每個晚上激增中沒有 Web 伺服器的流量。 網頁伺服器無法調整以處理增加的流量，在這些尖峰時間內，導致阻絕服務給客戶。 相同的尖峰小時流量多載會在歐洲與美國資料中心。 在一天中的其他時間，伺服器會處理流量，會遠低於其最大的功能。  
  
若要確保 contosogiftservices.com 客戶，從網站上取得回應靈敏的經驗，Contoso 禮物服務想要某些都柏林流量重新導向都柏林; 下午 6 點與下午 9 點之間的西雅圖 」 的應用程式伺服器而且他們想要某些的西雅圖流量重新導向至下午 6 點與下午 9 點，在西雅圖都柏林應用程式伺服器。  
  
下圖說明此案例。  
  
![天 DNS 原則範例的時間](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>根據時間日期的運作方式如何智慧型 DNS 回應  
  
當時間為下午 6 點與下午 9 點，每個地理位置之間的天 DNS 原則設定的 DNS 伺服器的 DNS 伺服器會執行下列作業。  
  
- 它會收到包含本機資料中心的 Web 伺服器的 IP 位址的前四個查詢的解答。  
- 回答第五個的查詢，它會收到包含遠端資料中心的 Web 伺服器的 IP 位址。   
  
這個以原則為基礎的行為來卸載 20%的本機 Web 伺服器的流量負載，到遠端 Web 伺服器加/減速本機應用程式伺服器上的負擔，並改善客戶的網站效能。  
  
在離峰時間，DNS 伺服器會執行基礎一般地理位置流量管理。 此外，將查詢傳送北美或歐洲以外的位置的 DNS 用戶端 DNS 伺服器流量進行負載平衡在西雅圖和都柏林的資料中心。  
  
當在 DNS 中設定多個 DNS 原則時，它們是已排序的集合的規則，並處理 DNS 優先順序從高到最低優先順序。 DNS 會使用第一個符合的情況下，包括當日時間原則。 基於這個理由，更具體的原則應該有較高的優先順序。 如果您建立的原則一天的時間，並授與他們的原則清單的高優先順序，DNS 會處理，並會先使用這些原則，如果它們符合 DNS 用戶端查詢與原則中定義之準則的參數。 如果兩者不符，DNS 向下移動的原則清單中要處理的預設原則，直到找到相符項目。  
  
如需有關原則類型和準則的詳細資訊，請參閱[DNS 原則概觀](../../dns/deploy/DNS-Policies-Overview.md)。  
  
### <a name="bkmk_how1"></a>如何設定 DNS 原則進行智慧型 DNS 回應的時間為基礎  
若要設定的一天的應用程式負載平衡以時間為基礎的查詢回應的 DNS 原則，您必須執行下列步驟。  
  
- [建立 DNS 用戶端的子網路](#bkmk_subnets)  
- [建立區域範圍](#bkmk_zscopes)  
- [將記錄新增至區域範圍](#bkmk_records)  
- [建立 DNS 原則](#bkmk_policies)  
  
>[!NOTE]
>您必須有權管理您想要設定區域的 DNS 伺服器上執行這些步驟。 中的成員資格**DnsAdmins**，或同等權限，才能執行下列程序。  
  
下列各節提供詳細的設定指示。  
  
>[!IMPORTANT]
>下列各節包含 Windows PowerShell 命令範例包含許多參數的範例值。 請確定這些命令列中的範例值取代是適用於您的部署，然後再執行這些命令的值。  
  
#### <a name="bkmk_subnets"></a>建立 DNS 用戶端的子網路  
第一個步驟是識別的子網路或 IP 位址空間，您要將流量重新導向的區域。 例如，如果您想要將流量重新導向適用於美國和歐洲，您需要識別的子網路或 IP 位址空間，這些區域。  
  
您可以從異地 IP 對應來取得這項資訊。 根據這些地理 IP 散發套件，您必須建立 「 DNS 用戶端的子網路。 」 DNS 用戶端的子網路是從中查詢傳送至 DNS 伺服器的 IPv4 或 IPv6 子網路的邏輯群組。  
  
您可以使用下列 Windows PowerShell 命令來建立 DNS 用戶端的子網路。  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
#### <a name="bkmk_zscopes"></a>建立區域範圍  
用戶端子網路設定之後，您必須分割您想要重新導向至兩個不同的區域範圍，其流量一個範圍，您已設定 DNS 用戶端子網路的每個區域。  
  
比方說，如果您想要將 DNS 名稱 www.contosogiftservices.com 流量重新導向，您必須建立兩個不同的區域範圍 contosogiftservices.com 區域、 一個用於美國和歐洲的其中一個。  
  
區域範圍內是區域的唯一的執行個體。 DNS 區域可以有多個區域範圍，與包含它自己的 DNS 記錄集的每個區域範圍。 同一筆記錄中可以存在多個領域，具有不同 IP 位址或相同的 IP 位址。  
  
>[!NOTE]
>根據預設，在區域範圍存在於 DNS 區域。 此區域範圍作為區域，具有相同的名稱，此範圍上運作的舊版 DNS 作業。  
  
您可以使用下列 Windows PowerShell 命令來建立區域範圍。  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
#### <a name="bkmk_records"></a>將記錄新增至區域範圍  
現在，您必須新增代表 web 伺服器主機的兩個區域範圍的記錄。  
  
例如，在**SeattleZoneScope**，記錄<strong>www.contosogiftservices.com</strong>新增 IP 位址 192.0.0.1，位於西雅圖的資料中心。 同樣地，在**DublinZoneScope**，記錄<strong>www.contosogiftservices.com</strong>加上在都柏林的資料中心的 IP 位址 141.1.0.3  
  
您可以使用下列 Windows PowerShell 命令，將記錄新增至區域範圍。  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
當您新增一筆記錄中的預設範圍時，未包含 ZoneScope 參數。 這是將記錄新增至標準 DNS 區域相同。  
  
如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
#### <a name="bkmk_policies"></a>建立 DNS 原則  
建立子網路之後，資料分割 （區域範圍），而且您已新增記錄，您必須建立連接的子網路和資料分割的原則，以便中 DNS 用戶端子網路的其中一個來源的查詢時，會傳回查詢回應正確的範圍內的區域。 沒有任何原則所需的對應預設區域範圍。  
  
您可以設定這些 DNS 原則之後，DNS 伺服器行為如下所示：  
  
1. 歐洲的 DNS 用戶端在都柏林資料中心，其 DNS 查詢回應中收到的 Web 伺服器的 IP 位址。  
2. American DNS 用戶端在西雅圖資料中心，其 DNS 查詢回應中收到的 Web 伺服器的 IP 位址。  
3. 下午 6 點與 9 PM 之間都柏林，來自歐洲的用戶端之查詢的 20%會收到在西雅圖資料中心，其 DNS 查詢回應中 Web 伺服器的 IP 位址。  
4. 下午 6 點與下午 9 點，在西雅圖，之間 20%的美國的用戶端的查詢會收到在都柏林資料中心，其 DNS 查詢回應中的 Web 伺服器的 IP 位址。  
5. 從世界各地的其餘部分查詢的下半部接收西雅圖資料中心的 IP 位址和另一半接收都柏林資料中心的 IP 位址。  
  
  
您可以使用下列 Windows PowerShell 命令來建立 DNS 原則，DNS 用戶端的子網路連結和區域範圍。  
  
>[!NOTE]
>在此範例中，DNS 伺服器是 GMT 時區中，因此尖峰時間必須以對等的 GMT 時間表示的時段。  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
現在已設定 DNS 伺服器，以必要的 DNS 原則重新導向的地理位置和當日時間為基礎的流量。  
  
當 DNS 伺服器收到名稱解析查詢時，DNS 伺服器會評估 DNS 要求，根據設定的 DNS 原則中的欄位。 如果在 名稱解析要求的來源 IP 位址會符合任何原則，相關聯的區域範圍來回應查詢，並將使用者導向的地理位置最接近它們的資源。  
  
您可以建立數以千計的 DNS 原則根據您的流量管理需求，而且所有新的原則都會套用動態-不需要重新啟動 DNS 伺服器-在傳入的查詢。


