---
title: 使用 DNS 原則進行應用程式負載平衡
description: 本主題是 DNS 原則案例指南的 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dca60fc0e216b1b873bd4f94dd1b01174d80fc14
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446438"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>使用 DNS 原則進行應用程式負載平衡

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解如何設定 DNS 原則以執行應用程式負載平衡。

舊版的 Windows Server DNS 只提供負載平衡使用循環配置資源回應;但是，使用 Windows Server 2016 中 DNS 時，您可以設定應用程式負載平衡的 DNS 原則。

當您部署的應用程式的多個執行個體時，您可以使用 DNS 原則來平衡不同的應用程式執行個體，藉此以動態方式配置應用程式的 流量負載之間的流量負載。

## <a name="example-of-application-load-balancing"></a>應用程式負載平衡的範例

以下是如何使用 DNS 原則，以及在應用程式負載平衡的範例。

此範例使用虛構公司 Contoso 禮物服務-它會提供線上 gifing 服務，且具有名為網站**contosogiftservices.com**。

Contosogiftservices.com 網站裝載於多個資料中心各有不同的 IP 位址。

在北美地區，也就是 Contoso 禮物服務的主要市場，網站裝載於三個資料中心：芝加哥，IL 德克薩斯州達拉斯，華盛頓州西雅圖市。

西雅圖網頁伺服器具有最佳的硬體組態，而且可以處理兩倍的負載，做為其他兩個站台。 Contoso 禮物服務想要以下列方式導向的應用程式流量。

- 西雅圖 Web 伺服器包括更多資源，因此應用程式的用戶端的下半部會導向到此伺服器
- 一季的應用程式的用戶端會被導向至德克薩斯州達拉斯資料中心
- 一季的應用程式的用戶端會被導向至芝加哥，IL，資料中心

下圖說明此案例。

![DNS 應用程式負載平衡使用 DNS 原則](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>如何在應用程式的負載平衡的運作

設定之後 DNS 伺服器與應用程式的 DNS 原則載入平衡使用此範例案例中，DNS 伺服器回應的 50%的西雅圖 Web 伺服器位址，25%的時間與 Dallas Web 伺服器位址，並使用時間 25%的時間芝加哥 Web 伺服器位址。

因此對於每個 DNS 伺服器收到四個的查詢，它會回應兩個回應 Seattle 和每個代表 Dallas 和芝加哥。

負載平衡使用 DNS 原則可能有一個問題是由 DNS 用戶端和解析程式/LDNS，可能會干擾負載平衡，因為解析程式的用戶端不要傳送查詢到 DNS 伺服器的 DNS 記錄的快取。

您可以使用低的時間，以避免此行為的影響\-要\-Live \(TTL\) DNS 記錄，應該進行負載平衡的值。

### <a name="how-to-configure-application-load-balancing"></a>如何設定應用程式負載平衡

下列各節會示範如何設定應用程式負載平衡的 DNS 原則。

#### <a name="create-the-zone-scopes"></a>建立區域範圍

您必須先建立的範圍的區域 contosogiftservices.com 資料中心裝載的位置。

區域範圍內是區域的唯一的執行個體。 DNS 區域可以有多個區域範圍，與包含它自己的 DNS 記錄集的每個區域範圍。 同一筆記錄中可以存在多個領域，具有不同 IP 位址或相同的 IP 位址。

>[!NOTE]
>根據預設，在區域範圍存在於 DNS 區域。 此區域範圍作為區域，具有相同的名稱，此範圍上運作的舊版 DNS 作業。

您可以使用下列 Windows PowerShell 命令來建立區域範圍。
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

如需詳細資訊，請參閱[新增 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

#### <a name="bkmk_records"></a>將記錄新增至區域範圍

現在，您必須新增至多個區域範圍表示 web 伺服器主機的記錄。

在  **SeattleZoneScope**，您可以將位於西雅圖的資料中心記錄 www.contosogiftservices.com 192.0.0.1 的 IP 位址。

在  **ChicagoZoneScope**，您可以將相同的記錄新增\(www.contosogiftservices.com\)具有 IP 位址 182.0.0.1 芝加哥資料中心內。

同樣地，在**DallasZoneScope**，您可以新增一筆資料錄\(www.contosogiftservices.com\)具有 IP 位址 162.0.0.1 芝加哥資料中心內。

您可以使用下列 Windows PowerShell 命令，將記錄新增至區域範圍。
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

#### <a name="bkmk_policies"></a>建立 DNS 原則

您已建立資料分割 （區域範圍），您已新增記錄之後，您必須建立散發傳入的查詢這些領域中，讓 50%的 contosogiftservices.com 的查詢會回應的 IP 位址與 web 的 DNS 原則在芝加哥和達拉斯的資料中心之間平均分散在西雅圖資料中心和其餘的伺服器。

您可以使用下列 Windows PowerShell 命令來建立應用程式流量平衡這些三個資料中心之間的 DNS 原則。

>[!NOTE]
>在下面的運算式範例命令 – ZoneScope"SeattleZoneScope，2;ChicagoZoneScope，1;DallasZoneScope，1-設定 DNS 伺服器陣列，其中包含參數的組合\<ZoneScope\>，\<權數\>。
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  

您現在已成功建立提供應用程式的負載分散在三個不同的資料中心的 Web 伺服器的 DNS 原則。

您可以建立數以千計的 DNS 原則根據您的流量管理需求，而且所有新的原則都會套用動態-不需要重新啟動 DNS 伺服器-在傳入的查詢。
