---
title: 透過主要-次要部署使用地理位置流量管理的 DNS 原則
description: 本主題是 Windows Server 2016 DNS 原則案例指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a7836160fc7363ec3d7b2fb11e194db82970f9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406158"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>透過主要-次要部署使用地理位置流量管理的 DNS 原則

>適用於：Windows Server (半年度管道)、Windows Server 2016

當您的 DNS 部署同時包含主要和次要 DNS 伺服器時，您可以使用本主題來瞭解如何針對以地理位置為基礎的流量管理建立 DNS 原則。  

先前的案例是[使用 Dns 原則進行地理位置型流量管理與主伺服器，並](primary-geo-location.md)提供在主要 DNS 伺服器上針對地理位置型流量管理設定 DNS 原則的指示。 不過，在網際網路基礎結構中，DNS 伺服器會廣泛部署在主要-次要模型中，其中區域的可寫入複本會儲存在選取和保護的主伺服器上，而區域的唯讀複本則會保留在多個次要伺服器上。   
  
次要伺服器會使用「區域傳輸通訊協定授權傳輸」（AXFR）和「增量區域傳輸」（IXFR）來要求和接收包含主要 DNS 伺服器上之區域新變更的區域更新。   
  
> [!NOTE]
> 如需 AXFR 的詳細資訊，請參閱網際網路工程任務推動小組（IETF）[要求建議 5936](https://tools.ietf.org/rfc/rfc5936.txt)。 如需有關 IXFR 的詳細資訊，請參閱網際網路工程任務推動小組（IETF）[要求建議 1995](https://tools.ietf.org/html/rfc1995)。  
  
## <a name="primary-secondary-geo-location-based-traffic-management-example"></a>主要-次要地理位置型流量管理範例  
以下範例說明如何在主要-次要部署中使用 DNS 原則，以根據執行 DNS 查詢的用戶端實體位置來進行流量重新導向。  
  
這個範例使用兩個虛構公司-Contoso 雲端服務，提供 web 和網域裝載解決方案;和 Woodgrove 食物服務，可提供全球多個城市中的食物傳遞服務，以及具有名為 woodgrove.com 的網站。  
  
為了確保 woodgrove.com 客戶從其網站獲得回應式體驗，Woodgrove 想要將歐洲的用戶端導向歐洲資料中心，並將美國客戶導向至美國資料中心。 位於世界各地的客戶可以導向到其中一個資料中心。  
  
Contoso 雲端服務有兩個資料中心，一個位於美國，另一個位於歐洲，Contoso 會在其上裝載其食物訂購入口網站以供 woodgrove.com。  
  
Contoso DNS 部署包含兩部次要伺服器：**SecondaryServer1**，IP 位址為 10.0.0.2;和**SecondaryServer2**，並使用 IP 位址10.0.0.3。 這些次要伺服器會作為兩個不同區域中的名稱伺服器，SecondaryServer1 位於歐洲，而 SecondaryServer2 位於美國
  
**PrimaryServer** （IP 位址10.0.0.1）上有一個主要的可寫入區域複本，其中會進列區域變更。 透過一般區域轉送到次要伺服器，次要伺服器一律會保持最新狀態，而對 PrimaryServer 上的區域有任何新的變更。
  
下圖描述此案例。
  
![主要-次要地理位置型流量管理範例](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="how-the-dns-primary-secondary-system-works"></a>DNS 主要-次要系統的運作方式

當您在主要-次要 DNS 部署中部署以地理位置為基礎的流量管理時，請務必先瞭解一般主要-次要區域傳輸如何在學習區域範圍層級傳輸之前進行。 下列各節提供區域和區域範圍層級傳輸的相關資訊。  
  
- [DNS 主要-次要部署中的區域傳輸](#zone-transfers-in-a-dns-primary-secondary-deployment)  
- [DNS 主要-次要部署中的區域範圍層級傳輸](#zone-scope-level-transfers-in-a-dns-primary-secondary-deployment)  
  
### <a name="zone-transfers-in-a-dns-primary-secondary-deployment"></a>DNS 主要-次要部署中的區域傳輸

您可以使用下列步驟建立 DNS 主要-次要部署並同步處理區域。  
1. 當您安裝 DNS 時，主要區域會建立在主要 DNS 伺服器上。  
2. 在次要伺服器上，建立區域並指定主伺服器。   
3. 在主伺服器上，您可以在主要區域上將次要伺服器新增為受信任的次要複本。   
4. 次要區域會進行完整的區域轉送要求（AXFR）並接收區域的複本。   
5. 如有需要，主伺服器會將有關區域更新的通知傳送到次要伺服器。  
6. 次要伺服器會進行增量區域轉送要求（IXFR）。 因此，次要伺服器會與主伺服器保持同步。   
  
### <a name="zone-scope-level-transfers-in-a-dns-primary-secondary-deployment"></a>DNS 主要-次要部署中的區域範圍層級傳輸

流量管理案例需要額外的步驟，才能將區域分割成不同的區域範圍。 因此，必須執行其他步驟，才能將區域範圍內的資料傳輸到次要伺服器，以及將原則和 DNS 用戶端子網傳送到次要伺服器。   
  
設定具有主要和次要伺服器的 DNS 基礎結構之後，DNS 會使用下列程式自動執列區域範圍層級的傳輸。  
  
為確保區域範圍層級的傳輸，DNS 伺服器會使用 DNS （EDNS0） OPT RR 的擴充機制。 具有範圍的區域的所有區域轉送（AXFR 或 IXFR）要求都是源自于 EDNS0 OPT RR，其選項識別碼預設為 "65433"。 如需有關 EDNSO 的詳細資訊，請參閱 IETF[提出意見要求 6891](https://tools.ietf.org/html/rfc6891)。  
  
OPT RR 的值是正在傳送要求的區域範圍名稱。 當主要 DNS 伺服器從受信任的次要伺服器接收此封包時，它會將要求轉譯為該區域範圍的即將到來。   
  
如果主伺服器具有該區域範圍，它會以該範圍的傳輸（XFR）資料回應。 回應包含 OPT RR，其選項識別碼為 "65433"，值設定為相同的區域範圍。 次要伺服器會接收此回應、從回應中取出範圍資訊，並更新該區域的特定範圍。  
  
在此程式之後，主伺服器會維護一份受信任次要複本的清單，這些次要複本已傳送通知的區域範圍要求。   
  
針對區域範圍中的任何進一步更新，會使用相同的 OPT RR，將 IXFR 通知傳送到次要伺服器。 接收通知的區域範圍會使包含 OPT RR 的 IXFR 要求和上述相同的程式，如下所示。  
  
## <a name="how-to-configure-dns-policy-for-primary-secondary-geo-location-based-traffic-management"></a>如何設定主要-次要地理位置型流量管理的 DNS 原則

在開始之前，請確定您已完成將[DNS 原則用於以地理位置為基礎的流量管理](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md)主題中的所有步驟，並使用區域、區域範圍、Dns 用戶端子網和 dns 設定主要 DNS 伺服器策略.  
  
> [!NOTE]
> 本主題中將 DNS 用戶端子網、區域範圍和 DNS 原則從 DNS 主伺服器複製到 DNS 次要伺服器的指示，適用于您的初始 DNS 設定和驗證。 未來，您可能會想要變更主伺服器上的 DNS 用戶端子網、區域範圍和原則設定。 在此情況下，您可以建立自動化腳本，讓次要伺服器與主伺服器保持同步。  
  
若要設定主要-次要地理位置型查詢回應的 DNS 原則，您必須執行下列步驟。  
  
- [建立次要區域](#create-the-secondary-zones)  
- [在主要區域上設定區域轉移設定](#configure-the-zone-transfer-settings-on-the-primary-zone)  
- [複製 DNS 用戶端子網](#copy-the-dns-client-subnets)  
- [在次要伺服器上建立區域範圍](#create-the-zone-scopes-on-the-secondary-server)  
- [設定 DNS 原則](#configure-dns-policy)  
  
下列各節提供詳細的設定指示。  
  
> [!IMPORTANT]
> 下列各節包含範例 Windows PowerShell 命令，其中包含許多參數的範例值。 執行這些命令之前，請務必將這些命令中的範例值取代為適用于您的部署的值。  
> 
> 若要執行下列程式，必須要有**DnsAdmins**的成員資格或同等許可權。  
  
### <a name="create-the-secondary-zones"></a>建立次要區域

您可以建立要複寫至 SecondaryServer1 和 SecondaryServer2 之區域的次要複本（假設 Cmdlet 是從單一管理用戶端遠端執行）。   
  
例如，您可以在 SecondaryServer1 和 SecondarySesrver2 上建立 www.woodgrove.com 的次要複本。  
  
您可以使用下列 Windows PowerShell 命令來建立次要區域。  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

如需詳細資訊，請參閱[DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps)。  
  
### <a name="configure-the-zone-transfer-settings-on-the-primary-zone"></a>在主要區域上設定區域轉移設定

您必須設定主要區域設定，以便：

1. 允許從主伺服器到指定次要伺服器的區域傳輸。  
2. 主要伺服器會將區域更新通知傳送到次要伺服器。  
  
您可以使用下列 Windows PowerShell 命令，在主要區域上設定區域轉移設定。
  
> [!NOTE]
> 在下列範例命令中，參數 **-Notify**指定主伺服器會將有關更新的通知傳送至選取的次要複本清單。  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
如需詳細資訊，請參閱[add-dnsserverprimaryzone](https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps)。  
  
  
### <a name="copy-the-dns-client-subnets"></a>複製 DNS 用戶端子網

您必須將 DNS 用戶端子網從主伺服器複製到次要伺服器。
  
您可以使用下列 Windows PowerShell 命令，將子網複製到次要伺服器。
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

如需詳細資訊，請參閱[DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="create-the-zone-scopes-on-the-secondary-server"></a>在次要伺服器上建立區域範圍

您必須在次要伺服器上建立區域範圍。 在 DNS 中，區域範圍也會開始向主伺服器要求 XFRs。 對於主伺服器上的區域範圍進行任何變更，包含區域範圍資訊的通知會傳送到次要伺服器。 然後，次要伺服器就可以更新其區域範圍，並進行增量變更。  
  
您可以使用下列 Windows PowerShell 命令，在次要伺服器上建立區域範圍。  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

> [!NOTE]
> 在這些範例命令中，會包含 **-ErrorAction Ignore**參數，因為每個區域上都有預設區域範圍。 無法建立或刪除預設區域範圍。 管線會導致嘗試建立該範圍，而且將會失敗。 或者，您可以在兩個次要區域上建立非預設的區域範圍。  
  
如需詳細資訊，請參閱[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="configure-dns-policy"></a>設定 DNS 原則

建立子網、分割區（區域範圍）並新增記錄之後，您必須建立用來連接子網和分割區的原則，如此一來，當查詢來自其中一個 DNS 用戶端子網中的來源時，就會從傳回查詢回應區域的正確範圍。 對應預設區域範圍不需要任何原則。  
  
您可以使用下列 Windows PowerShell 命令來建立連結 DNS 用戶端子網和區域範圍的 DNS 原則。   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

如需詳細資訊，請參閱[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
現在，次要 DNS 伺服器會使用必要的 DNS 原則進行設定，以根據地理位置來重新導向流量。  
  
當 DNS 伺服器收到名稱解析查詢時，DNS 伺服器會根據所設定的 DNS 原則來評估 DNS 要求中的欄位。 如果名稱解析要求中的來源 IP 位址符合任何原則，則會使用相關聯的區域範圍來回應查詢，而使用者會被導向到地理位置最接近的資源。   
  
您可以根據您的流量管理需求來建立數以千計的 DNS 原則，而所有的新原則都會以動態方式套用，而不需要重新開機 DNS 伺服器上的傳入查詢。
