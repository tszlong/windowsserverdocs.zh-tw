---
title: 使用 Azure 雲端應用程式伺服器以時間為基礎的 DNS 回應
description: 瞭解如何使用 Azure Cloud App Server 根據一天中的時間來使用 DNS 回應。
manager: brianlic
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b90af11d4bb6f5c1f52699669a8eee4562a97d48
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904923"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>使用 Azure 雲端應用程式伺服器以時間為基礎的 DNS 回應

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何使用以一天時間為基礎的 DNS 原則，將應用程式流量分散到應用程式的不同地理位置分散式實例。

如果您想要將某個時區的流量導向至其他應用程式伺服器（例如，裝載于 Microsoft Azure 上的 Web 服務器）的其他時區，這種情況就很有用。 這可讓您在主伺服器以流量超載時，在尖峰時段內跨應用程式實例進行流量負載平衡。

> [!NOTE]
> 若要瞭解如何在不使用 Azure 的情況下使用智慧型 DNS 回應的 DNS 原則，請參閱 [根據一天中的時間，將 Dns 原則用於智慧型 Dns 回應](./dns-tod-intelligent.md)。

## <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day-with-azure-cloud-app-server"></a>以 Azure Cloud App Server 當天時間為基礎的智慧型 DNS 回應範例

以下範例說明如何使用 DNS 原則，根據一天中的時間來平衡應用程式流量。

此範例使用一家虛構的公司 Contoso 禮物服務，其可透過其網站 contosogiftservices.com，在全球各地提供線上贈與解決方案。

Contosogiftservices.com 網站只裝載于西雅圖 (的單一內部部署資料中心，並具有公用 IP 192.68.30.2) 。

DNS 伺服器也位於內部部署資料中心。

有了最新的業務激增，contosogiftservices.com 每天有更多的訪客，而某些客戶回報了服務可用性問題。

Contoso 禮物服務會執行網站分析，並探索每一晚上下午6點到下午9點之間的當地時間，與西雅圖網頁伺服器的流量有激增。 Web 服務器無法調整以處理在這些尖峰時間增加的流量，而導致客戶拒絕服務。

為了確保 contosogiftservices.com 客戶從網站獲得回應式體驗，Contoso 禮物服務會決定在這段期間內，它會租借 Microsoft Azure 上的虛擬機器 \( VM， \) 以裝載其 Web 服務器的複本。

Contoso 禮物服務會從 Azure 取得 VM 的公用 IP 位址 (192.68.31.44) 並開發自動化，以在 Azure 上每天的 5-10 Azure 上部署 Web 服務器，並允許一小時的應變期間。

> [!NOTE]
> 如需有關 Azure Vm 的詳細資訊，請參閱 [虛擬機器檔](https://azure.microsoft.com/documentation/services/virtual-machines/)

DNS 伺服器會使用區域範圍和 DNS 原則進行設定，因此每天的 5-9 PM 之間，系統會將30% 的查詢傳送至在 Azure 中執行的 Web 服務器實例。

下圖描述此案例。

![當日時間回應的 DNS 原則](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)

## <a name="how-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server-works"></a>以 Azure App Server 的當日時間為基礎的智慧型 DNS 回應如何運作

本文示範如何設定 DNS 伺服器以使用兩個不同的應用程式伺服器 IP 位址來回應 DNS 查詢-一部 web 伺服器位於西雅圖，另一個則位於 Azure 資料中心。

設定新的 DNS 原則時，如果是以西雅圖下午6點到9點的尖峰時間為基礎，DNS 伺服器就會將每個 DNS 回應的70傳送給包含西雅圖 Web 服務器 IP 位址的用戶端，而每% 的 DNS 回應的每%因此，會將用戶端流量導向至新的 Azure Web 服務器，並防止西雅圖 Web 服務器超載。

在每天的其他時間，系統會進行一般查詢處理，並從包含內部部署資料中心內 web 伺服器記錄的預設區域範圍傳送回應。

Azure 記錄上10分鐘的 TTL 可確保在從 Azure 移除 VM 之前，LDNS 快取中的記錄已過期。 這種規模調整的優點之一，是您可以將 DNS 資料保留在內部部署環境，並視需要將您的資料擴充至 Azure。

## <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server"></a>如何根據使用 Azure App Server 的當日時間，設定智慧型 DNS 回應的 DNS 原則

若要設定「每日時間的應用程式負載平衡」查詢回應的 DNS 原則，您必須執行下列步驟。

- [建立區域範圍](#create-the-zone-scopes)
- [將記錄新增至區域範圍](#add-records-to-the-zone-scopes)
- [建立 DNS 原則](#create-the-dns-policies)

> [!NOTE]
> 您必須在您想要設定之區域的授權 DNS 伺服器上執行這些步驟。 若要執行下列程式，必須要有 DnsAdmins 的成員資格或同等許可權。

下列各節提供詳細的設定指示。

> [!IMPORTANT]
> 下列各節包含範例 Windows PowerShell 包含許多參數範例值的命令。 執行這些命令之前，請務必將這些命令中的範例值取代為適合您部署的值。


### <a name="create-the-zone-scopes"></a>建立區域範圍

區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，每個區域範圍都包含一組專屬的 DNS 記錄。 相同記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。

> [!NOTE]
> 依預設，DNS 區域上會有區域範圍。 此區域範圍與區域具有相同的名稱，且舊版 DNS 作業可在此範圍內運作。

您可以使用下列範例命令來建立區域範圍，以裝載 Azure 記錄。

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

如需詳細資訊，請參閱 [新增-DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope)

### <a name="add-records-to-the-zone-scopes"></a>將記錄新增至區域範圍
下一步是將代表 Web 服務器主機的記錄新增到區域範圍中。

在 AzureZoneScope 中，會使用 IP 位址192.68.31.44 （位於 Azure 公用雲端中）來新增 [記錄] www.contosogiftservices.com。

同樣地，在預設區域範圍 \( contosogiftservices.com 中 \) ， \( \) 會使用在西雅圖內部部署資料中心內執行之 Web 服務器的 IP 位址192.68.30.2 來新增 [記錄] www.contosogiftservices.com。

在下列第二個 Cmdlet 中，不會包含– ZoneScope 參數。 因此，會在預設 ZoneScope 中加入記錄。

此外，Azure Vm 記錄的 TTL 會保持 600s (10 分鐘) ，讓 LDNS 不會將它快取一段較長的時間，這會干擾負載平衡。 此外，Azure Vm 提供1個額外的一小時做為應變，以確保即使是具有快取記錄的用戶端都能夠解析。

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

如需詳細資訊，請參閱 [DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord)。

### <a name="create-the-dns-policies"></a>建立 DNS 原則
建立區域範圍之後，您可以建立 DNS 原則，將傳入的查詢分散到這些範圍，以便進行下列動作。

1. 從下午6點到下午9點，30% 的用戶端會在 DNS 回應的 Azure 資料中心內收到 Web 服務器的 IP 位址，而70% 的用戶端會收到西雅圖內部部署 Web 服務器的 IP 位址。
2. 在其他所有情況下，所有用戶端都會收到西雅圖內部部署 Web 服務器的 IP 位址。

當日的時間必須以 DNS 伺服器的當地時程表示。

您可以使用下列範例命令來建立 DNS 原則。

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

如需詳細資訊，請參閱 [DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy)。

現在 DNS 伺服器已設定必要的 DNS 原則，以根據一天的時間將流量重新導向至 Azure Web 服務器。

請注意運算式：

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00”
`

此運算式會將 DNS 伺服器設定為 ZoneScope 和權陣列合，以指示 DNS 伺服器在每個時間的每個伺服器上傳送每美分的西雅圖 Web 70 伺服器的 IP 位址，而每秒傳送 Azure Web 服務器三十的 IP 位址。

您可以根據您的流量管理需求建立數千個 DNS 原則，而且會動態套用所有新原則，而不需要重新開機 DNS 伺服器上的傳入查詢。
