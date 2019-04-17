---
title: 使用應用程式負載平衡 DNS 原則
description: 本主題是 DNS 原則案例指南適用於 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d156c258b971c45bf1c4c20739440bd5cc9e239f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing"></a>使用應用程式負載平衡 DNS 原則

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何設定執行的應用程式負載平衡 DNS 原則。

之前版本的 Windows Server DNS 只提供負載平衡所使用的回應循環配置資源。但是，您可以在 Windows Server 2016、 dns 設定 DNS 負載平衡] 應用程式的原則。

當您完成部署多個應用程式時，您可以使用 DNS 原則，以平衡不同的應用程式執行個體，藉此動態應用程式的配置流量載入間流量載入。

## <a name="example-of-application-load-balancing"></a>應用程式負載平衡的範例

以下是您如何使用的應用程式負載平衡 DNS 原則的範例。

此範例中使用虛構公司-Contoso 禮品服務-提供 online gifing 服務，並其網站名為**contosogiftservices.com**。

多個能源各有不同的 IP 位址裝載 contosogiftservices.com 網站。

北美洲，以 Contoso 禮品服務主要市場，三個能源裝載網站： 伊利諾州芝加哥、 達拉斯、 傳送與西雅圖縣。

西雅圖網頁伺服器具有最佳硬體設定，並為兩個網站倍載入處理。 Contoso 禮品服務想要以下列方式導向的應用程式流量。

- 西雅圖網頁伺服器包含更多資源，因為一半的應用程式的用導向這個伺服器
- Datacenter 達拉斯、 傳送導向的應用程式的用一季
- 伊利諾州芝加哥，datacenter 導向的應用程式的用一季

下圖描述此案例。

![DNS 應用程式負載平衡 DNS 原則](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>應用程式如何負載平衡運作

您已經設定好之後 DNS 原則的應用程式的 DNS 伺服器載入平衡使用此案例，DNS 伺服器回應 50%的西雅圖網頁伺服器位址，25%的達拉斯網頁伺服器位址，以及 25%的芝加哥網址伺服器的時間。

因此的 DNS 伺服器接收每個四個查詢，它看具有兩個回應西雅圖和一個每個達拉斯和芝加哥。

負載平衡 DNS 原則的一個可能的問題，是 DNS 記錄 DNS client 並解析日 LDNS，這可能會干擾負載平衡，因為 client 或解析不要傳送查詢 DNS 伺服器的快取。

您可以使用較低的 Time\ to\ 動態 \(TTL\) 值應負載平衡 DNS 記錄降低效果的這個問題。

### <a name="how-to-configure-application-load-balancing"></a>如何設定應用程式負載平衡

下列區段會顯示如何設定 DNS 負載平衡] 應用程式的原則。

#### <a name="create-the-zone-scopes"></a>建立區域範圍

您必須先建立領域的區域 contosogiftservices.com 資料中心裝載的位置。

時區領域是區域的唯一執行個體。 DNS 區域可以有多個區域領域，與每個包含 DNS 記錄它自己設定的區域範圍。 相同記錄可能會出現在多個領域，以不同的 IP 位址或相同的 IP 位址。

>[!NOTE]
>根據預設，區域領域存在於 DNS 區域。 這個區域領域作為區域，具有相同的名稱，並在這個領域中工作舊版 DNS 作業。

您可以使用下列的 Windows PowerShell 命令來建立區域範圍。
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

如需詳細資訊，請查看[新增-DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

####<a name="bkmk_records"></a>若要的區域領域加入資料

現在，您必須新增到區域領域代表網頁伺服器主機記錄。

在**SeattleZoneScope**，您可以將使用碼表進行 www.contosogiftservices.com IP 位址 192.0.0.1，位於西雅圖資料中心。

在**ChicagoZoneScope**，您可以新增的 IP 位址 182.0.0.1 相同使用碼表進行 \(www.contosogiftservices.com\) 芝加哥資料中心。

同樣地，在**DallasZoneScope**，您可以新增的 IP 位址 162.0.0.1 記錄 \(www.contosogiftservices.com\) 芝加哥資料中心。

您可以使用下列 Windows PowerShell 命令若要的區域領域加入資料。
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

如需詳細資訊，請查看[新增-DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx)。

####<a name="bkmk_policies"></a>建立 DNS 原則

您所建立的磁碟分割 （區域領域） 並新增了記錄之後，您必須建立 DNS 原則，連入查詢分配這些範圍，讓 50%的查詢 contosogiftservices.com 交給西雅圖 datacenter 中的網頁伺服器的 IP 位址和其他同樣視訊光碟之間芝加哥與達拉斯資料中心。

您可以使用下列的 Windows PowerShell 命令建立的餘額下列三種資料中心上的應用程式資料傳輸 DNS 原則。

>[!NOTE]
>在下列運算式範例命令 – ZoneScope 」 SeattleZoneScope，2。ChicagoZoneScope，1。DallasZoneScope，1 」 設定的 DNS 伺服器的包含參數組合陣列 \ < ZoneScope\，> \ < weight\ >。
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW – -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

如需詳細資訊，請查看[新增-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。  

您現在已成功建立 DNS 原則，以提供應用程式負載平衡三個不同的資料中心中的網頁伺服器上。

您可以建立數千 DNS 原則根據您的資料傳輸管理的需求，且所有的新原則已經套用動態-不需要重新 DNS 伺服器-連入查詢。