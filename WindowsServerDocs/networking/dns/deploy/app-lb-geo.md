---
title: 使用應用程式負載平衡的地理位置感知 DNS 原則
description: 本主題是 DNS 原則案例指南適用於 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7637d927c7b22b83053e7f9100b07581c11bafc0
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>使用應用程式負載平衡的地理位置感知 DNS 原則

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何設定 DNS 負載平衡應用程式的地理位置認知的原則。

本指南，在前一個主題[使用 DNS 原則的應用程式負載平衡](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb)、使用範例虛構公司-Contoso 禮品服務-online 贈送所提供的服務，和這已網站命名 contosogiftservices.com。Contoso 禮品服務負載平衡 online Web 應用程式之間北美位於西雅圖縣、伊利諾州芝加哥，以及達拉斯、傳送的資料中心中的伺服器。

>[!NOTE]
>建議您，您熟悉的主題[適用於應用程式負載平衡使用 DNS 原則](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb)之前，請先執行此案例中的指示操作。

本主題使用相同虛構公司和網路基礎結構做為基礎的新的範例部署包含地理位置感知。

在此範例中，以 Contoso 禮品服務成功全球的展開他們卡。

北美洲類似，公司現在已在歐洲資料中心裝載的網頁伺服器。

設定應用程式負載平衡適用於歐洲的資料中心 DNS 原則實作在美國，類似的方式使用應用程式流量分配，愛爾蘭都柏林，阿姆斯特丹、 荷蘭、 中或其他地方找到的網頁伺服器要 Contoso 禮品服務 DNS 系統管理員。

DNS 系統管理員也可以從其他位置世界同樣分散在所有的資料中心中的所有查詢。

中的下一個區段中，您可以了解如何達成類似的目標，以 Contoso DNS 您自己的網路系統管理員的。

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>如何設定平衡的地理位置感知應用程式

下列區段會顯示如何設定的應用程式負載平衡的地理位置感知 DNS 原則。

>[!IMPORTANT]
>以下的各節包含包含許多參數值範例範例 Windows PowerShell 命令。 請確認值是適用於您的部署，執行下列命令之前，先取代範例值這些命令列中。

###<a name="bkmk_clientsubnets"></a>建立 DNS Client 子網路

您第一次必須找出子網路的 IP 位址空間北美洲、 歐洲的地區。

您可以從地理 IP 「 地圖 」 來取得此資訊。 依據這些地理 IP 散發，您必須建立 DNS Client 子網路。

DNS Client 子網路是 IPv4 或 IPv6 子網路，查詢會傳送至 DNS 伺服器的邏輯群組。

若要建立 DNS Client 子網路，您可以使用下列的 Windows PowerShell 命令。 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
如需詳細資訊，請查看[新增-DnsServerClientSubnet](https://technet.microsoft.com/library/mt126261.aspx)。

###<a name="bkmk_zscopes2"></a>建立區域範圍

Client 子網路中的位置之後，您必須到不同的區域的範圍，datacenter 針對每個分割區 contosogiftservices.com。

時區領域是區域的唯一執行個體。 DNS 區域可以有多個區域領域，與每個包含 DNS 記錄它自己設定的區域範圍。 相同記錄可能會出現在多個領域，以不同的 IP 位址或相同的 IP 位址。

>[!NOTE]
>根據預設，區域領域存在於 DNS 區域。 這個區域領域作為區域，具有相同的名稱，並在這個領域中工作舊版 DNS 作業。

在應用程式負載平衡前一個案例示範如何設定的資料中心有三種區域範圍北美地區。

使用下列命令，您可以建立兩個更多區域範圍，其中每個的都柏林和阿姆斯特丹資料中心。 

您可以新增相同時區中的三個現有北美地區時區領域這些區域範圍，而無須進行任何變更。 此外，在建立這些區域範圍之後，您不需要重新開機您的 DNS 伺服器。

您可以使用下列的 Windows PowerShell 命令來建立區域範圍。

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

如需詳細資訊，請查看[新增-DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records2"></a>若要的區域領域加入資料

現在，您必須新增到區域領域代表網頁伺服器主機記錄。

在前一個案例中新增的記錄地區資料中心。 您可以使用下列 Windows PowerShell 命令若要適用於歐洲的資料中心區域領域加入資料。
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

如需詳細資訊，請查看[新增-DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx)。

###<a name="bkmk_policies2"></a>建立 DNS 原則

您所建立的磁碟分割 （區域領域） 並新增了記錄之後，您必須建立 DNS 原則，連入查詢分配這些範圍。

針對此範例中，在不同的資料中心中的應用程式伺服器查詢 distribution 符合下列條件。

1. DNS 查詢收到時來源北美 client 子網路中的 DNS 回應 50%指到西雅圖資料中心，25%的回應指向芝加哥資料中心，達拉斯 datacenter 指向剩餘 25%的回應。
2. 來源歐洲 client 子網路中收到 DNS 查詢時，50%的 DNS 回應都柏林 datacenter，指向 [和 50%的 DNS 回應指向阿姆斯特丹資料中心。
3. 查詢來自其他地方的世界中，當 DNS 回應分散在所有的五個資料中心。

您可以使用下列 Windows PowerShell 命令來執行這些 DNS 原則。

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

如需詳細資訊，請查看[新增-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。

您現在已成功建立 DNS 原則，以提供應用程式負載平衡位於多個大陸上五個不同的資料中心中的網頁伺服器上。

您可以建立數千 DNS 原則根據您的資料傳輸管理的需求，且所有的新原則已經套用動態-不需要重新 DNS 伺服器-連入查詢。
