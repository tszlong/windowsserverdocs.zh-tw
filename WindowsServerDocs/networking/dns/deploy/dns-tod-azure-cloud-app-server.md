---
title: 根據一天中 Azure 時間 DNS 回應雲端伺服器的應用程式
description: 本主題是 DNS 原則案例指南適用於 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3255d3fe29f6a7dda821183fa4352964cc230a5f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>根據一天中 Azure 時間 DNS 回應雲端伺服器的應用程式

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何在應用程式的流量分配，使用 DNS 原則一天的時間為基礎的應用程式的不同分散執行個體。 

本案例可用於您想来直接傳輸到其他應用程式的伺服器，例如網頁伺服器裝載 Microsoft Azure、上位於其他時區時區中的位置。 這可讓您在應用程式執行個體的山峰載入餘額流量主要伺服器資料傳輸多載句點這樣的時間。 

>[!NOTE]
>若要了解如何使用的智慧型 DNS 回應 DNS 原則，不使用 Azure，請查看[使用 DNS 原則智慧 DNS 回應根據一天的時間在](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md)。 

## <a name="bkmk_azureexample"></a>根據一天的 Azure 雲端應用程式伺服器的時間智慧 DNS 回應的範例

以下是如何使用 DNS 原則，以根據一天的時間餘額應用程式流量的範例。

此範例中使用一虛構家公司，以 Contoso 禮品服務，透過他們的網站，contosogiftservices.com 全球提供 online 贈送方案。 

只有在單一先 datacenter（的公用 IP 192.68.30.2) 西雅圖裝載 contosogiftservices.com 網站。 

先 datacenter 也位於 DNS 伺服器。 

在商務用最近突波，contosogiftservices.com 已經有更多的訪客每日，針對部分服務的可用性問題報告。 

Contoso 禮品服務執行網站分析，並探索之間 6 PM 和 9 PM 本地時間每個夜晚突波中已資料傳輸到西雅圖網頁伺服器。 Web 伺服器無法縮放處理流量增加在這些山峰的時數，導致阻斷服務來針對。 

若要確保 contosogiftservices.com 針對從網站取得回應式的經驗，以 Contoso 禮品服務決定期間時段它將會租借 virtual 電腦 \(VM\) Microsoft Azure 裝載一份其網頁伺服器上。  

Contoso 禮品服務取得 VM (192.68.31.44 從 Azure 公用 IP 位址）並開發自動化部署網頁伺服器每日 Azure 上之間 5-10 PM，允許一個意外小時。

>[!NOTE]
>如需 Azure Vm 的詳細資訊，請查看[電腦虛擬文件](https://azure.microsoft.com/documentation/services/virtual-machines/) 

這樣的查詢 30%傳送到執行 Azure 中的網頁伺服器的執行個體之間 5-9 下午每日，區域範圍和 DNS 原則設定的 DNS 伺服器。

下圖描述此案例。

![回應天的時間 DNS 原則](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="bkmk_azurehow"></a>如何智慧 DNS 回應根據一天 Azure 與的時間伺服器的應用程式的運作方式
 
這篇文章示範如何設定將會解答 DNS 查詢具有兩個不同的應用程式伺服器 IP 位址的 DNS 伺服器一個網頁伺服器是西雅圖，以及另一個是在 Azure 資料中心。

新的 DNS 原則尖峰的 6 至 9 下午西雅圖為基礎的設定之後, DNS 伺服器會傳送 seventy 每一分戶端包含西雅圖網頁伺服器的 IP 位址的 DNS 回應的和通知三十每一分的 DNS 回應戶端包含 Azure 網頁伺服器的 IP 位址藉此引導 client 資料傳輸到新的 Azure Web 伺服器，並負荷防止西雅圖網頁伺服器。 

其他時候，天的一般查詢處理程序發生，並且包含記錄網頁伺服器上場所 datacenter 中的預設區域領域從傳送回覆。 

在 Azure 記錄 10 分鐘 TTL 可確保記錄已過期從 LDNS 快取 Azure 從移除 VM 之前。 其中一個優點這類縮放比例是您可以讓您 DNS 資料先，並保留縮放比例出 Azure 需要要求。

## <a name="bkmk_azureconfigure"></a>如何設定智慧 DNS 回應根據一天 Azure 的 App 伺服器的時間 DNS 原則
若要設定時間的一天應用程式負載平衡查詢回應 DNS 原則，您必須執行下列步驟。


- [建立區域範圍](#bkmk_zscopes)
- [若要的區域領域加入資料](#bkmk_records)
- [建立 DNS 原則](#bkmk_policies)


>[!NOTE]
>您必須是針對您想要設定的區域授權的 DNS 伺服器上執行這些步驟。 資格 DnsAdmins，或等，才能執行下列程序。 

下列章節提供詳細的設定指示操作。

>[!IMPORTANT]
>以下的各節包含包含許多參數值範例範例 Windows PowerShell 命令。 請確認值是適用於您的部署，執行下列命令之前，先取代範例值這些命令列中。 


### <a name="bkmk_zscopes"></a>建立區域範圍
時區領域是區域的唯一執行個體。 DNS 區域可以有多個區域領域，與每個包含 DNS 記錄它自己設定的區域範圍。 相同記錄可能會出現在多個領域，以不同的 IP 位址或相同的 IP 位址。 

>[!NOTE]
>根據預設，區域領域存在於 DNS 區域。 這個區域領域作為區域，具有相同的名稱，並在這個領域中工作舊版 DNS 作業。 

您可以使用下列命令範例建立裝載 Azure 記錄領域區域。

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

如需詳細資訊，請查看[新增-DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

### <a name="bkmk_records"></a>若要的區域領域加入資料
下一個步驟是加入到區域領域代表網頁伺服器主機資料。 

AzureZoneScope，記錄 www.contosogiftservices.com 已新增的 IP 位址 192.68.31.44，在公用 Azure 雲端中。 

同樣地，在 [預設區域範圍 \(contosogiftservices.com\)，記錄 \(www.contosogiftservices.com\) 會執行西雅圖先 datacenter 中的網頁伺服器的 IP 位址 192.68.30.2 加入。

在第二個下列 cmdlet，不包含 – ZoneScope 參數。 因此，記錄加入 ZoneScope 預設值。 

此外，Azure Vm 的記錄 TTL 會保留在 600s（10 分鐘），以便 LDNS 不要快取它較長的時間-干擾負載平衡。 此外，Azure Vm 可確保的快取記錄甚至戶端無法解析應變額外 1 小時的。

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

如需詳細資訊，請查看[新增-DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx)。  

### <a name="bkmk_policies"></a>建立 DNS 原則 
建立區域範圍之後，您可以建立 DNS 原則，連入查詢分配這些範圍，下列，就會發生。

1. 下午 6 至 9 PM 每天，從的用 30%時，收到網頁伺服器的 IP 位址 DNS 因應日光 Azure 資料中心中的用 70%收到西雅圖先網頁伺服器的 IP 位址。
2. 其他時候，用所有戶端都收到西雅圖先網頁伺服器的 IP 位址。

必須以當地的 DNS 伺服器的時間來表示一天的時間。

若要建立的 DNS 原則，您可以使用下列命令範例。

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW –-ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

如需詳細資訊，請查看[新增-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。  
  
立即所需的 DNS 原則，將根據一天的時間伺服器 Azure 網路流量被設定 DNS 伺服器。 

請注意運算式：

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

這個運算式 ZoneScope 和減重組合指示 DNS 伺服器的時間，而傳送 Azure 網頁伺服器的 IP 位址的時間通知三十每一分 seventy 每一分傳送西雅圖網頁伺服器的 IP 位址設定 DNS 伺服器。

您可以建立數千 DNS 原則根據您的資料傳輸管理的需求，且所有的新原則已經套用動態-不需要重新 DNS 伺服器-連入查詢。
