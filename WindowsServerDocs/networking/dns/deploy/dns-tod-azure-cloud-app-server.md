---
title: 使用 Azure 雲端應用程式伺服器以時間為基礎的 DNS 回應
description: 本主題是 DNS 原則案例指南的 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ed6ac2ebc8839d0e7ecee682d7644251f8a59381
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829069"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>使用 Azure 雲端應用程式伺服器以時間為基礎的 DNS 回應

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解如何將應用程式流量分散到不同地理位置分散的應用程式執行個體中，使用 DNS 原則為基礎的當日時間。 

這個案例是在您想要將流量導向至替代的應用程式伺服器，例如 Web 伺服器裝載於 Microsoft Azure 上位於另一個時區中的一個時區中的情況下很有用。 這可讓您將應用程式執行個體之間的平衡流量負載尖峰期間時間的週期，您的主要伺服器的流量多載。 

>[!NOTE]
>若要了解如何使用 DNS 原則進行智慧型 DNS 回應，而不需使用 Azure，請參閱[使用 DNS 原則智慧型 DNS 回應時間的以天為基礎](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md)。 

## <a name="bkmk_azureexample"></a>Azure 雲端應用程式伺服器的同一天的時間為基礎的智慧型 DNS 回應的範例

以下是如何使用 DNS 原則以一天的時間為基礎的應用程式流量平衡的範例。

這個範例會使用一個虛構的公司 Contoso 禮物卡服務，可透過其網站在全球各地提供線上贈與解決方案，contosogiftservices.com。 

只有在西雅圖 （具有公用 IP 192.68.30.2) 的單一內部部署資料中心裝載 contosogiftservices.com 網站。 

DNS 伺服器也位於內部部署資料中心。 

與商務最近激增，contosogiftservices.com 較高數目的訪客每一天，而且某些客戶已回報服務可用性問題。 

Contoso 禮物卡服務執行站台分析，並探索下午 6 點與下午 9 點的當地時間之間的每個晚上沒有激增到西雅圖 Web 伺服器流量。 網頁伺服器無法調整以處理增加的流量，在這些尖峰時間內，導致阻絕服務給客戶。 

為了確保 contosogiftservices.com 客戶網站上取得回應靈敏的經驗，Contoso 禮物卡服務會決定在這些時間內就將租用虛擬機器\(VM\)在 Microsoft Azure 來裝載一份其 Web 伺服器上.  

Contoso 禮物卡服務 VM (192.68.31.44) 從 Azure 取得的公用 IP 位址，並開發自動化功能，每日在 Azure 上之間 5-下午 10 點，以便在一個小時的緊急應變期間部署 Web 伺服器。

>[!NOTE]
>如需有關 Azure Vm 的詳細資訊，請參閱[虛擬機器文件](https://azure.microsoft.com/documentation/services/virtual-machines/) 

DNS 伺服器會設定具有區域範圍和 DNS 原則中，如此之間 5-9 PM 每一天、 30%的查詢會傳送至 Azure 中執行的 Web 伺服器執行個體。

下圖說明此案例。

![一天的回應時間的 DNS 原則](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="bkmk_azurehow"></a>如何智慧型 DNS 回應為基礎的當日時間和 Azure 應用程式伺服器的運作方式
 
這篇文章會示範如何設定 DNS 伺服器回應 DNS 查詢具有兩個不同的應用程式伺服器 IP 位址-一部 web 伺服器是在西雅圖，另一個是 Azure 資料中心。

完成新的 DNS 原則為基礎的尖峰時間的下午 6 點至下午 9 點在西雅圖的設定之後, DNS 伺服器會傳送包含西雅圖 Web 伺服器的 IP 位址的用戶端的 DNS 回應的 seventy %和 30%的用戶端的 DNS 回應nts 包含 Azure Web 伺服器的 IP 位址、 藉此用戶端將流量導向至新的 Azure Web 伺服器，並避免西雅圖 Web 伺服器超過負載。 

一天，其他所有時間進行一般的查詢處理的位置，並從預設區域範圍，其中包含 web 伺服器，在內部部署資料中心內的記錄傳送回應。 

Azure 記錄的 10 分鐘的 TTL 可確保 VM 從 Azure 移除之前，已從 LDNS 快取過期記錄。 這種調整的優點是您可以保留您 DNS 在內部資料，並讓向外擴充至 Azure 依需求。

## <a name="bkmk_azureconfigure"></a>如何設定 DNS 原則進行智慧型 DNS 回應為基礎的當日時間和 Azure 的應用程式伺服器
若要設定的一天的應用程式負載平衡以時間為基礎的查詢回應的 DNS 原則，您必須執行下列步驟。


- [建立區域範圍](#bkmk_zscopes)
- [將記錄新增至區域範圍](#bkmk_records)
- [建立 DNS 原則](#bkmk_policies)


>[!NOTE]
>您必須有權管理您想要設定區域的 DNS 伺服器上執行這些步驟。 DnsAdmins，或是對等成員資格，才能執行下列程序。 

下列各節提供詳細的設定指示。

>[!IMPORTANT]
>下列各節包含 Windows PowerShell 命令範例包含許多參數的範例值。 請確定這些命令列中的範例值取代是適用於您的部署，然後再執行這些命令的值。 


### <a name="bkmk_zscopes"></a>建立區域範圍
區域範圍內是區域的唯一的執行個體。 DNS 區域可以有多個區域範圍，與包含它自己的 DNS 記錄集的每個區域範圍。 同一筆記錄中可以存在多個領域，具有不同 IP 位址或相同的 IP 位址。 

>[!NOTE]
>根據預設，在區域範圍存在於 DNS 區域。 此區域範圍作為區域，具有相同的名稱，此範圍上運作的舊版 DNS 作業。 

您可以使用下列範例命令建立裝載 Azure 記錄的區域範圍。

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

如需詳細資訊，請參閱[新增 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records"></a>將記錄新增至區域範圍
下一個步驟是新增到區域範圍表示 Web 伺服器主機的記錄。 

在 AzureZoneScope，記錄 www.contosogiftservices.com 新增 IP 位址 192.68.31.44，其位於 Azure 公用雲端。 

同樣地，在預設區域領域\(contosogiftservices.com\)，記錄\(www.contosogiftservices.com\)加入在西雅圖內部執行的 Web 伺服器的 IP 位址 192.68.30.2資料中心。

在下列第二個 cmdlet 中，您可能未包含要 – ZoneScope 參數。 因為這個緣故，在預設 ZoneScope 新增記錄。 

此外，適用於 Azure Vm 記錄的 TTL 會保留在 600s （10 分鐘），以便 LDNS 不會快取它較長的時間-這會干擾負載平衡。 此外，Azure Vm 有 1 個額外小時作為應變計劃以確保即使快取的記錄用戶端就能解決。

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  

### <a name="bkmk_policies"></a>建立 DNS 原則 
建立區域範圍之後，您可以建立將傳入的查詢分散到這些範圍，以便發生下列情況的 DNS 原則。

1. 從每日下午 9 點到下午 6 點，30%的用戶端的 Web 伺服器的 IP 位址在 DNS 回應中，在 Azure 資料中心時收到 70%的用戶端接收西雅圖的內部部署 Web 伺服器的 IP 位址。
2. 在所有其他情況下，所有用戶端收到西雅圖的內部部署 Web 伺服器的 IP 位址。

在一天的時間必須以 DNS 伺服器的當地時間表示。

若要建立的 DNS 原則，您可以使用下列範例命令。

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
現在已設定 DNS 伺服器，以必要的 DNS 原則，將流量重新導向至 Azure Web 伺服器為基礎的當日時間。 

請注意運算式：

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

這個運算式會結合 ZoneScope 和權數，它會指示傳送西雅圖 Web 伺服器的 IP 位址 seventy %的時間，同時傳送時間的 30%的 Azure Web 伺服器的 IP 位址的 DNS 伺服器設定 DNS 伺服器。

您可以建立數以千計的 DNS 原則根據您的流量管理需求，而且所有新的原則都會套用動態-不需要重新啟動 DNS 伺服器-在傳入的查詢。
