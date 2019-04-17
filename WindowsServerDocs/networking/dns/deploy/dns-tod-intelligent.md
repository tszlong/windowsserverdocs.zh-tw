---
title: 使用 DNS 原則智慧 DNS 回應根據一天的時間
description: 本主題是 DNS 原則案例指南適用於 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 46821daff4a0046bf78d7f56dc7c5deabcc437e4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>使用 DNS 原則智慧 DNS 回應根據一天的時間

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何在應用程式的流量分配，使用 DNS 原則一天的時間為基礎的應用程式的不同分散執行個體。  
  
本案例可用於您想来直接傳輸到其他應用程式的伺服器，例如 [位於另一部時區中的網頁伺服器一個時區中的位置。 這可讓您在應用程式執行個體的山峰載入餘額流量主要伺服器資料傳輸多載句點這樣的時間。   
  
### <a name="bkmk_example1"></a>根據一天的時間智慧 DNS 回應的範例  
以下是如何使用 DNS 原則，以根據一天的時間餘額應用程式流量的範例。  
  
此範例中使用一虛構家公司，以 Contoso 禮品服務，透過他們的網站，contosogiftservices.com 全球提供 online 贈送方案。   
  
在兩個耗電量、西雅圖（北美）中，並在都柏林（歐洲）的另一個裝載 contosogiftservices.com 網站。 傳送地理位置注意回應，使用 DNS 原則設定的 DNS 伺服器。 在商務用最近突波，contosogiftservices.com 已經有更多的訪客每日，針對部分服務的可用性問題報告。  
  
Contoso 禮品服務執行網站分析，並探索之間 6 PM 和 9 PM 本地時間每個夜晚突波中已流量的網頁伺服器。 Web 伺服器無法縮放處理流量增加在這些山峰的時數，導致阻斷服務來針對。 相同的山峰小時流量多載是中歐與美國資料中心。 在其他一天的時間，伺服器處理流量磁碟區的遠低於他們最大的功能。  
  
若要確保 contosogiftservices.com 針對從網站取得回應式的經驗，以 Contoso 禮品服務想要重新導向至都柏林; 9 PM 下午 6 點之間的西雅圖應用程式伺服器的一些都柏林流量並想要重新導向某些西雅圖流量 6 PM 和 9 PM 西雅圖之間的都柏林應用程式伺服器。  
  
下圖描述此案例。  
  
![範例天 DNS 原則的時間](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>如何智慧 DNS 回應根據一天運作的時間  
  
設定使用時間的一天 DNS 原則，6 下午之間 9 PM 在每個地理位置的 DNS 伺服器時會執行下列的 DNS 伺服器。  
  
- 解答前四個查詢收到 datacenter 區域中的網頁伺服器的 IP 位址。  
- 解答五查詢收到遠端 datacenter 中的網頁伺服器的 IP 位址。   
  
這項原則為主行為卸載 20 每一分本機的網頁伺服器流量負載遠端網頁伺服器簡化負擔應用程式本機伺服器，並改善針對網站的效能。  
  
峰的 DNS 伺服器執行一般地理-位置型流量管理。 此外 DNS 用傳送查詢從北美地區或歐洲以外的任何位置的 DNS 伺服器負載平衡流量，在西雅圖和 Dublin 資料中心。  
  
多個 DNS 原則是設定在 DNS，他們排序的組規則，，便會處理 DNS 最高優先順序的最低的優先順序。 DNS 使用的第一個原則符合環境，包括一天的時間。 基於這個原因，更特定原則應該會有較高優先順序。 如果您建立的原則天的時間，將它們設定為高優先順序原則的清單中，DNS 處理並是否符合您的 DNS client 查詢和原則中定義條件參數第一次使用這些原則。 如果不符合，DNS 下移原則清單處理預設的原則，直到您找到符合。  
  
如需有關原則類型條件，請查看[DNS 原則概觀](../../dns/deploy/DNS-Policies-Overview.md)。  
  
### <a name="bkmk_how1"></a>如何設定智慧 DNS 回應根據一天的時間 DNS 原則  
若要設定時間的一天應用程式負載平衡查詢回應 DNS 原則，您必須執行下列步驟。  
  
- [建立 DNS Client 子網路](#bkmk_subnets)  
- [建立區域範圍](#bkmk_zscopes)  
- [若要的區域領域加入資料](#bkmk_records)  
- [建立 DNS 原則](#bkmk_policies)  
  
>[!NOTE]
>您必須是針對您想要設定的區域授權的 DNS 伺服器上執行這些步驟。 資格在**DnsAdmins**，或等，才能執行下列程序。  
  
下列章節提供詳細的設定指示操作。  
  
>[!IMPORTANT]
>以下的各節包含包含許多參數值範例範例 Windows PowerShell 命令。 請確認值是適用於您的部署，執行下列命令之前，先取代範例值這些命令列中。  
  
#### <a name="bkmk_subnets"></a>建立 DNS Client 子網路  
找出子網路的 IP 位址，您想要重新導向流量地區空間是第一個步驟。 例如，如果您想要將流量美國和歐洲重新導向，您需要找出子網路的 IP 位址空間這些地區。  
  
您可以從地理 IP 「 地圖 」 來取得此資訊。 依據這些地理 IP 散發，您必須建立」DNS Client 子」。 DNS Client 子網路是 IPv4 或 IPv6 子網路，查詢會傳送至 DNS 伺服器的邏輯群組。  
  
若要建立 DNS Client 子網路，您可以使用下列的 Windows PowerShell 命令。  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
如需詳細資訊，請查看[新增-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
#### <a name="bkmk_zscopes"></a>建立區域範圍  
Client 子網路設定之後，您必須磁碟分割的流量您想要重新導向至兩種不同的區域範圍，領域 DNS Client 子網路，您所設定的區域。  
  
例如，如果您想要重新導向之 DNS 名稱 www.contosogiftservices.com 流量，您必須建立兩種不同的區域範圍 contosogiftservices.com 區域，另一個用於美國和歐洲的其中一個。  
  
時區領域是區域的唯一執行個體。 DNS 區域可以有多個區域領域，與每個包含 DNS 記錄它自己設定的區域範圍。 相同記錄可能會出現在多個領域，以不同的 IP 位址或相同的 IP 位址。  
  
>[!NOTE]
>根據預設，區域領域存在於 DNS 區域。 這個區域領域作為區域，具有相同的名稱，並在這個領域中工作舊版 DNS 作業。  
  
您可以使用下列的 Windows PowerShell 命令來建立區域範圍。  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
如需詳細資訊，請查看[新增-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
#### <a name="bkmk_records"></a>若要的區域領域加入資料  
現在，您必須將記錄代表網頁伺服器主機成兩個區域範圍。  
  
例如，在**SeattleZoneScope**，記錄**www.contosogiftservices.com**的 IP 位址 192.0.0.1，在西雅圖資料中心中新增了。 同樣地，在**DublinZoneScope**，記錄**www.contosogiftservices.com**的 IP 位址 141.1.0.3 都柏林 datacenter 中新增了  
  
您可以使用下列 Windows PowerShell 命令若要的區域領域加入資料。  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
當您新增記錄預設範圍中不包含 ZoneScope 參數。 這是標準 DNS 時區新增記錄相同。  
  
如需詳細資訊，請查看[新增-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
#### <a name="bkmk_policies"></a>建立 DNS 原則  
子網路建立後的磁碟分割（區域領域），而且您已新增記錄、查詢回應 DNS client 子網路的來源查詢時，會傳回正確的範圍的區域的您必須建立連接子網路和的磁碟分割的原則。 不原則所需的對應區域預設範圍。  
  
這些 DNS 原則設定之後，DNS 伺服器運作方式如下：  
  
1. 歐洲 DNS 用都柏林 datacenter 他們 DNS 查詢因應日光中收到的網頁伺服器的 IP 位址。  
2. 美國 DNS 用西雅圖 datacenter 他們 DNS 查詢因應日光中收到的網頁伺服器的 IP 位址。  
3. 6 PM，Dublin 9 PM 之間從歐洲的查詢 20%，請在西雅圖 datacenter 他們 DNS 查詢因應日光收到網頁伺服器的 IP 位址。  
4. 下午 6，在西雅圖 9 PM 之間 20%，從美國查詢會收到網頁伺服器的 IP 位址都柏林 datacenter 他們 DNS 查詢因應日光中。  
5. 半部來自全球的其餘部分查詢接收西雅圖 datacenter 的 IP 位址和其他半接收都柏林 datacenter 的 IP 位址。  
  
  
您可以使用下列的 Windows PowerShell 命令來建立 DNS 原則連結 DNS Client 子網路，以及區域範圍。  
  
>[!NOTE]
>在此範例中，DNS 伺服器處於 GMT 的時區，因此澳地區的山峰小時必須以相同的 GMT 時間表示時段。  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
如需詳細資訊，請查看[新增-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
現在 DNS 伺服器會以重新導向流量地理位置與的時間型所需的 DNS 原則設定。  
  
當 DNS 伺服器接收名稱解析查詢時、DNS 伺服器評估 DNS 要求針對 DNS 原則設定中的欄位。 如果名稱解析要求來源 IP 位址比對任何原則，相關的區域範圍用來回應查詢，和使用者導向它們地理位置最接近的資源。  
  
您可以建立數千 DNS 原則根據您的資料傳輸管理的需求，且所有的新原則已經套用動態-不需要重新 DNS 伺服器-連入查詢。


