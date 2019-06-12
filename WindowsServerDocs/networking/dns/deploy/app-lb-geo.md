---
title: 使用 DNS 原則進行具有地理位置感知的應用程式負載平衡
description: 本主題是 DNS 原則案例指南的 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9f76163e6b064ac3225ab4d755afd548e1cb720b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446417"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>使用 DNS 原則進行具有地理位置感知的應用程式負載平衡

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解如何設定負載平衡應用程式具有地理位置感知的 DNS 原則。

本指南中上, 一個主題[使用 DNS 原則進行應用程式負載平衡](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb)、 使用的虛構公司 Contoso 禮物服務-提供線上贈與範例服務，且具有名為的網站contosogiftservices.com。 Contoso 禮物 Services 負載平衡線上 Web 應用程式伺服器位於華盛頓州西雅圖、 芝加哥，IL 和德克薩斯州達拉斯北美的資料中心之間。

>[!NOTE]
>我們建議您，您已經熟悉它的主題[使用 DNS 原則進行應用程式負載平衡](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb)之前在此案例中執行的指示。

本主題使用的同一個虛構的公司和網路基礎結構做為基礎的新的範例部署，其中包含地理位置感知。

在此範例中，Contoso 禮物服務成功地擴展其目前狀態在全球各地。

類似於北美地區，公司現在已在歐洲資料中心內裝載的 web 伺服器。

Contoso 禮物服務的 DNS 系統管理員想要設定應用程式負載平衡至美國境內的 DNS 原則實作類似的方式與應用程式流量分配給 Web 伺服器位於歐洲資料中心愛爾蘭的都柏林、 阿姆斯特丹，荷蘭，和其他位置。

DNS 系統管理員也會想從其他位置中所有的資料中心之間平均分散在世界的所有查詢。

在後續小節中，您可以了解如何達成類似的目標，Contoso DNS 系統管理員，在您自己的網路上的作業。

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>如何設定應用程式負載平衡與地理位置感知

下列各節會示範如何設定應用程式負載平衡與地理位置感知的 DNS 原則。

>[!IMPORTANT]
>下列各節包含 Windows PowerShell 命令範例包含許多參數的範例值。 請確定這些命令列中的範例值取代是適用於您的部署，然後再執行這些命令的值。

### <a name="bkmk_clientsubnets"></a>建立 DNS 用戶端的子網路

您必須先識別的子網路或 IP 位址空間的北美洲與歐洲地區。

您可以從異地 IP 對應來取得這項資訊。 根據這些地理 IP 散發套件，您必須建立 DNS 用戶端子網路。

DNS 用戶端的子網路是從中查詢傳送至 DNS 伺服器的 IPv4 或 IPv6 子網路的邏輯群組。

您可以使用下列 Windows PowerShell 命令來建立 DNS 用戶端的子網路。 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。

### <a name="bkmk_zscopes2"></a>建立區域範圍

用戶端子網路已備妥之後，您必須分割區域 contosogiftservices.com 為不同的區域範圍，每個資料中心。

區域範圍內是區域的唯一的執行個體。 DNS 區域可以有多個區域範圍，與包含它自己的 DNS 記錄集的每個區域範圍。 同一筆記錄中可以存在多個領域，具有不同 IP 位址或相同的 IP 位址。

>[!NOTE]
>根據預設，在區域範圍存在於 DNS 區域。 此區域範圍作為區域，具有相同的名稱，此範圍上運作的舊版 DNS 作業。

在應用程式負載平衡先前的案例示範如何在北美地區設定的資料中心的三個區域範圍。

您可以使用下列命令建立兩個更多區域範圍，分別用於在都柏林和阿姆斯特丹資料中心。 

您可以加入相同的區域中的三個現有的北美區域範圍中這些區域範圍，而不需要任何變更。 此外，您建立這些區域範圍之後，您不需要重新啟動您的 DNS 伺服器。

您可以使用下列 Windows PowerShell 命令來建立區域範圍。

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

如需詳細資訊，請參閱[新增 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records2"></a>將記錄新增至區域範圍

現在，您必須新增至多個區域範圍表示 web 伺服器主機的記錄。

前一個案例中，未加入 America 資料中心的記錄。 您可以使用下列 Windows PowerShell 命令，將記錄新增至歐洲資料中心的區域範圍。
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

### <a name="bkmk_policies2"></a>建立 DNS 原則

您已建立資料分割 （區域範圍），您已新增記錄之後，您必須建立將傳入的查詢分散到這些領域的 DNS 原則。

例如，跨不同資料中心內的應用程式伺服器的查詢分佈會符合下列準則。

1. 當 DNS 查詢會從來源接收在北美的用戶端的子網路，DNS 回應的 50%點到西雅圖資料中心中，指向芝加哥資料中心的 25%的回應，並剩下的 25%的回應指向 Dallas 資料中心。
2. 從歐洲的用戶端的子網路中的來源收到之 DNS 查詢時，DNS 回應的 50%指向都柏林資料中心，然後指向阿姆斯特丹資料中心的 DNS 回應的 50%。
3. 當查詢來自其他地方的世界中時，DNS 回應會分散到所有五個資料中心中。

您可以使用下列 Windows PowerShell 命令來實作這些 DNS 原則。

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

您現在已成功建立提供應用程式的負載平衡位於多個大陸上五個不同的資料中心的 Web 伺服器之間的 DNS 原則。

您可以建立數以千計的 DNS 原則根據您的流量管理需求，而且所有新的原則都會套用動態-不需要重新啟動 DNS 伺服器-在傳入的查詢。
