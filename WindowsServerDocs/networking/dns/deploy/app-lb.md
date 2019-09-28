---
title: 使用 DNS 原則進行應用程式負載平衡
description: 本主題是 Windows Server 2016 DNS 原則案例指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 356c61c2cc5b60f43a69f17966c97f3c69d05cda
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356035"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>使用 DNS 原則進行應用程式負載平衡

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何設定 DNS 原則來執行應用程式負載平衡。

舊版的 Windows Server DNS 只會使用迴圈配置資源回應來提供負載平衡;但是在 Windows Server 2016 中的 DNS，您可以設定應用程式負載平衡的 DNS 原則。

當您已部署應用程式的多個實例時，您可以使用 DNS 原則來平衡不同應用程式實例之間的流量負載，進而動態配置應用程式的流量負載。

## <a name="example-of-application-load-balancing"></a>應用程式負載平衡的範例

以下是如何使用 DNS 原則進行應用程式負載平衡的範例。

這個範例會使用一個虛構公司-Contoso 禮物服務-提供線上 gifing 服務，以及具有名為**contosogiftservices.com**的網站。

Contosogiftservices.com 網站裝載于多個資料中心，每個都有不同的 IP 位址。

在北美洲（Contoso 禮物服務的主要市場）中，此網站裝載于三個資料中心：芝加哥、IL、達拉斯、德克薩斯州和西雅圖，WA。

西雅圖 Web 服務器具有最佳的硬體設定，而且可以處理與其他兩個網站一樣多的負載。 Contoso 禮物服務想要以下列方式導向應用程式流量。

- 因為西雅圖網頁伺服器包含更多資源，所以應用程式的一半用戶端會被導向到這部伺服器
- 應用程式用戶端的一季會導向至達拉斯、德克薩斯州資料中心
- 應用程式用戶端的一季會導向芝加哥、IL、datacenter

下圖描述此案例。

![使用 DNS 原則進行 DNS 應用程式負載平衡](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>應用程式負載平衡的運作方式

使用此範例案例設定 DNS 伺服器與應用程式負載平衡的 DNS 原則之後，DNS 伺服器會以西雅圖 Web 服務器位址回應 50% 的時間，這是達拉斯 Web 服務器位址的 25%，而時間的 25%芝加哥 Web 服務器位址。

因此，針對每四個 DNS 伺服器收到的查詢，它會以西雅圖的兩個回應回應，而每個都適用于達拉斯和芝加哥。

Dns 原則的負載平衡有一個可能的問題，就是 DNS 用戶端和解析程式/LDNS 快取 DNS 記錄，這可能會干擾負載平衡，因為用戶端或解析程式不會傳送查詢到 DNS 伺服器。

您可以使用低時間 @ no__t-0to @ no__t-1Live \(TTL @ no__t-3 值，針對應進行負載平衡的 DNS 記錄，減輕此行為的影響。

### <a name="how-to-configure-application-load-balancing"></a>如何設定應用程式負載平衡

下列各節說明如何設定應用程式負載平衡的 DNS 原則。

#### <a name="create-the-zone-scopes"></a>建立區域範圍

您必須先為其裝載所在的資料中心建立區域 contosogiftservices.com 的範圍。

區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，其中每個區域範圍都包含自己的一組 DNS 記錄。 相同的記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。

>[!NOTE]
>根據預設，區域範圍會存在於 DNS 區域中。 此區域範圍與區域具有相同的名稱，且舊版 DNS 作業會在此範圍內運作。

您可以使用下列 Windows PowerShell 命令來建立區域範圍。
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

如需詳細資訊，請參閱[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

#### <a name="bkmk_records"></a>將記錄新增至區域範圍

現在您必須將代表 web 伺服器主機的記錄新增到區域範圍中。

在**SeattleZoneScope**中，您可以使用位於西雅圖資料中心的 IP 位址192.0.0.1 來新增記錄 www.contosogiftservices.com。

在**ChicagoZoneScope**中，您可以使用芝加哥資料中心的 IP 位址182.0.0.1 來新增相同的記錄 \(www. contosogiftservices .com @ no__t-2。

同樣地，在**DallasZoneScope**中，您可以在芝加哥資料中心內，新增記錄 \(www. contosogiftservices .com @ no__t-2 與 IP 位址162.0.0.1。

您可以使用下列 Windows PowerShell 命令，將記錄新增至區域範圍。
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

如需詳細資訊，請參閱[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

#### <a name="bkmk_policies"></a>建立 DNS 原則

建立分割區（區域範圍）並新增記錄之後，您必須建立可將傳入查詢分散到這些範圍的 DNS 原則，以便將 contosogiftservices.com 的 50% 查詢以 Web 的 IP 位址回應西雅圖資料中心內的伺服器，其餘部分則平均分散于芝加哥和達拉斯的資料中心。

您可以使用下列 Windows PowerShell 命令來建立將應用程式流量平衡到這三個資料中心的 DNS 原則。

>[!NOTE]
>在下面的範例命令中，expression – ZoneScope "SeattleZoneScope，2;ChicagoZoneScope，1;DallasZoneScope，1 "將 DNS 伺服器設定為包含參數組合的陣列 \<ZoneScope @ no__t-1，\<weight @ no__t-3。
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

如需詳細資訊，請參閱[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  

您現在已成功建立 DNS 原則，以在三個不同資料中心的 Web 服務器之間提供應用程式負載平衡。

您可以根據您的流量管理需求來建立數以千計的 DNS 原則，而所有的新原則都會以動態方式套用，而不需要重新開機 DNS 伺服器上的傳入查詢。
