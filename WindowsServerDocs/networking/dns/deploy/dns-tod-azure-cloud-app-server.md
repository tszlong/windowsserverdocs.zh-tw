---
title: 使用 Azure 雲端應用程式伺服器以時間為基礎的 DNS 回應
description: 本主題是 Windows Server 2016 DNS 原則案例指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 04e4d33f6c5894a59547e84a6066d3af04f80a9b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964104"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>使用 Azure 雲端應用程式伺服器以時間為基礎的 DNS 回應

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何使用以當天時間為基礎的 DNS 原則，將應用程式流量分散到應用程式的不同地理位置。

當您想要將一個時區的流量導向至替代的應用程式伺服器（例如，Microsoft Azure 上所裝載的 Web 服務器）時，此案例很有用。 這可讓您在主伺服器以流量超載時的尖峰時段，對應用程式實例之間的流量進行負載平衡。

> [!NOTE]
> 若要瞭解如何使用 DNS 原則來取得智慧型 DNS 回應，而不使用 Azure，請參閱[根據一天的時間使用 Dns 原則來進行智慧型 Dns 回應](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md)。

## <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day-with-azure-cloud-app-server"></a>以 Azure 雲端應用程式伺服器當天的時間為基礎的智慧型 DNS 回應範例

以下範例示範如何使用 DNS 原則，根據一天的時間來平衡應用程式流量。

這個範例使用一個虛構的公司 Contoso 禮物服務，透過其網站 contosogiftservices.com 提供全球各地的線上贈與解決方案。

Contosogiftservices.com 網站僅裝載于具有公用 IP 192.68.30.2) 的西雅圖 (中的單一內部部署資料中心。

DNS 伺服器也位於內部部署資料中心。

在最新的企業營運中，contosogiftservices.com 每天會有更多的訪客，而且有些客戶回報了服務可用性問題。

Contoso 禮物服務會執行網站分析，併發現在當地時間下午6點到下午9：00，西雅圖 Web 服務器的流量激增。 網頁伺服器無法調整以在這些尖峰時間處理增加的流量，因而導致客戶拒絕服務。

為了確保 contosogiftservices.com 客戶從網站獲得回應式體驗，Contoso 禮物服務會決定在這段期間內，會租用虛擬機器 VM， \( \) Microsoft Azure 裝載其 Web 服務器的複本。

Contoso 禮品服務會從 Azure 取得 VM 的公用 IP 位址 (192.68.31.44) 並開發自動化，以在每天 5-10 PM 的 Azure 上部署 Web 服務器，允許一小時的應變週期。

> [!NOTE]
> 如需 Azure Vm 的詳細資訊，請參閱[虛擬機器檔](https://azure.microsoft.com/documentation/services/virtual-machines/)

DNS 伺服器是以區域範圍和 DNS 原則進行設定，因此每天下午5-9，會將30% 的查詢傳送至 Azure 中執行的 Web 服務器實例。

下圖描述此案例。

![當日時間回應的 DNS 原則](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)

## <a name="how-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server-works"></a>以 Azure App Server 的一天時間為基礎，智慧型 DNS 回應的運作方式

本文示範如何使用兩個不同的應用程式伺服器 IP 位址來設定 DNS 伺服器來回答 DNS 查詢-一部 web 伺服器位於西雅圖，另一個則位於 Azure 資料中心。

根據西雅圖的尖峰時數下午6點至9點設定新的 DNS 原則之後，DNS 伺服器會針對包含西雅圖 Web 服務器 IP 位址的用戶端，將70的每個 DNS 回應傳送到每美分，而對於包含 Azure Web 服務器 IP 位址的用戶端，則會將每個 dns 回應的百分之30。會將用戶端流量導向至新的 Azure Web 服務器，並防止西雅圖網頁伺服器超載。

在每天的其他所有時間，會進行一般查詢處理，而回應會從預設區域範圍傳送，其中包含內部部署資料中心內 web 伺服器的記錄。

Azure 記錄上10分鐘的 TTL 可確保從 LDNS 快取中的記錄過期後，才會從 Azure 移除 VM。 這類調整的其中一個優點是您可以將 DNS 資料保留在內部部署環境，並視需要繼續向外擴充至 Azure。

## <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server"></a>如何根據 Azure App 伺服器的當日時間，設定智慧型 DNS 回應的 DNS 原則

若要針對每日時間以應用程式負載平衡為基礎的查詢回應設定 DNS 原則，您必須執行下列步驟。

- [建立區域範圍](#create-the-zone-scopes)
- [將記錄新增至區域範圍](#add-records-to-the-zone-scopes)
- [建立 DNS 原則](#create-the-dns-policies)

> [!NOTE]
> 您必須在您想要設定之區域的授權 DNS 伺服器上執行這些步驟。 若要執行下列程式，必須要有 DnsAdmins 的成員資格或同等許可權。

下列各節提供詳細的設定指示。

> [!IMPORTANT]
> 下列各節包含範例 Windows PowerShell 命令，其中包含許多參數的範例值。 執行這些命令之前，請務必將這些命令中的範例值取代為適用于您的部署的值。


### <a name="create-the-zone-scopes"></a>建立區域範圍

區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，其中每個區域範圍都包含自己的一組 DNS 記錄。 相同的記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。

> [!NOTE]
> 根據預設，區域範圍會存在於 DNS 區域中。 此區域範圍與區域具有相同的名稱，且舊版 DNS 作業會在此範圍內運作。

您可以使用下列範例命令來建立區域範圍，以裝載 Azure 記錄。

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

如需詳細資訊，請參閱[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a>將記錄新增至區域範圍
下一個步驟是將代表 Web 服務器主機的記錄新增到區域範圍中。

在 AzureZoneScope 中，會使用位於 Azure 公用雲端中的 IP 位址192.68.31.44 新增記錄 www.contosogiftservices.com。

同樣地，在 [預設區域範圍] \( contosogiftservices.com 中 \) ， \( 會加入記錄 www.contosogiftservices.com， \) 其中包含在西雅圖內部部署資料中心內執行之 Web 服務器的 IP 位址192.68.30.2。

在下面的第二個 Cmdlet 中，不包含– ZoneScope 參數。 因此，會在預設 ZoneScope 中新增記錄。

此外，Azure Vm 記錄的 TTL 會保留在 600s (10 分鐘) ，讓 LDNS 不會將它快取一段較長的時間，這會干擾負載平衡。 此外，Azure Vm 提供1個額外的時間作為應變，以確保即使是具有快取記錄的用戶端都可以解析。

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

如需詳細資訊，請參閱[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

### <a name="create-the-dns-policies"></a>建立 DNS 原則
建立區域範圍之後，您可以建立 DNS 原則，將傳入的查詢分散到這些範圍，以便發生下列情況。

1. 每日下午6點至9點，30% 的用戶端會在 DNS 回應中接收 Azure 資料中心內 Web 服務器的 IP 位址，而70% 的用戶端則會收到西雅圖內部部署 Web 服務器的 IP 位址。
2. 所有其他情況下，所有用戶端都會收到西雅圖內部部署 Web 服務器的 IP 位址。

一天中的時間必須以 DNS 伺服器的當地時程表示。

您可以使用下列範例命令來建立 DNS 原則。

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

如需詳細資訊，請參閱[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

現在，DNS 伺服器已設定必要的 DNS 原則，以根據一天的時間將流量重新導向至 Azure Web 服務器。

請注意運算式：

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00”
`

此運算式會使用 ZoneScope 和權陣列合來設定 DNS 伺服器，以指示 DNS 伺服器在一段時間內傳送西雅圖 Web 服務器70的 IP 位址，並在一段時間內傳送 Azure Web 服務器的 IP 位址三十美元。

您可以根據您的流量管理需求來建立數以千計的 DNS 原則，而所有的新原則都會以動態方式套用，而不需要重新開機 DNS 伺服器上的傳入查詢。
