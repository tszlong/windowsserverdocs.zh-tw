---
title: 透過主要-次要部署使用地理位置流量管理的 DNS 原則
description: 本主題是 DNS 原則案例指南的 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6869ee5f39f1719a3c71025207ef9ffe740492ff
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266782"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>透過主要-次要部署使用地理位置流量管理的 DNS 原則

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解如何建立地理位置流量管理的 DNS 原則，當您的 DNS 部署包含主要和次要 DNS 伺服器。  

前述案例中，[地理位置與主要伺服器，根據流量管理的使用 DNS 原則](primary-geo-location.md)，提供的主要 DNS 伺服器上設定的地理位置流量管理的 DNS 原則的指示。 在網際網路基礎結構，不過，DNS 伺服器會廣泛地部署在主要-次要模型中，選取且安全的主要伺服器上儲存區域的可寫入複本，而區域的唯讀複本會保留在多部次要伺服器上。   
  
次要伺服器會使用區域傳輸通訊協定授權傳輸 (AXFR) 及增量區域轉送 (IXFR) 要求並接收區域更新包含新的主要 DNS 伺服器上區域的變更。   
  
>[!NOTE]
>如需 AXFR 的詳細資訊，請參閱網際網路工程任務推動小組 (IETF)[要求的註解 5936](https://tools.ietf.org/rfc/rfc5936.txt)。 如需 IXFR 的詳細資訊，請參閱網際網路工程任務推動小組 (IETF)[要求的註解 1995年](https://tools.ietf.org/html/rfc1995)。  
  
## <a name="primary-secondary-geo-location-based-traffic-management-example"></a>主要-次要地理位置型流量管理範例  
以下是如何，您可以在主要-次要部署中使用 DNS 原則以達到根據用戶端，執行 DNS 查詢的實體位置的流量重新導向的範例。  
  
這個範例會使用兩個虛構公司服務-Contoso 的雲端服務提供 web 與網域託管解決方案，為 Woodgrove 餐飲業，它會提供在多個城市的食物傳遞服務全球各地且具有網站 woodgrove.com。  
  
若要確保 woodgrove.com 客戶，從其網站取得回應的體驗，Woodgrove 想歐洲的用戶端導向到歐洲資料中心和美國的用戶端導向至在美國資料中心。 在其他地方位於世界各地的客戶可以導向至其中一個資料中心。  
  
Contoso 的雲端服務有兩個資料中心，一個在美國，另一個在歐洲，賴以 Contoso 裝載其排序 woodgrove.com 的入口網站的食物。  
  
Contoso DNS 部署包含兩個次要伺服器：**SecondaryServer1**，使用 IP 位址 10.0.0.2; 以及**SecondaryServer2**，使用 IP 位址 10.0.0.3。 這些次要伺服器做為名稱伺服器，在兩個不同區域中，使用位於歐洲和 SecondaryServer2 位於美國的 SecondaryServer1
  
在沒有可寫入的主要區域複本**PrimaryServer** （IP 位址為 10.0.0.1），進行區域變更。 使用一般的區域轉送到次要伺服器，次要伺服器一律是最新的任何新的變更至 PrimaryServer 的區域。
  
下圖說明此案例。
  
![主要-次要地理位置型流量管理範例](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="how-the-dns-primary-secondary-system-works"></a>DNS 主要-次要系統的運作方式

當您部署在主要-次要 DNS 部署的地理位置流量管理時，務必了解如何正常進行之前了解區域範圍層級傳輸的傳輸的主要-次要區域。 下列各節提供區域和區域範圍層級傳輸的資訊。  
  
- [在 DNS 主要-次要部署的區域轉送](#zone-transfers-in-a-dns-primary-secondary-deployment)  
- [傳輸中的 DNS 主要-次要部署的區域範圍層級](#zone-scope-level-transfers-in-a-dns-primary-secondary-deployment)  
  
### <a name="zone-transfers-in-a-dns-primary-secondary-deployment"></a>在 DNS 主要-次要部署的區域轉送

您可以建立 DNS 主要-次要部署，並執行下列步驟同步區域。  
1. 當您安裝 DNS 時，主要 DNS 伺服器上建立主要區域。  
2. 在次要伺服器上，建立區域並指定主要伺服器。   
3. 主要伺服器上，您可以新增次要伺服器作為受信任的次要資料庫，在主要區域。   
4. 次要區域進行完整的區域傳輸要求 (AXFR)，而且收到的區域複本。   
5. 需要時，主要伺服器會將通知傳送的次要伺服器有關區域的更新。  
6. 次要伺服器進行增量區域傳輸要求 (IXFR)。 因為這個緣故，次要伺服器都能保持與主要伺服器同步。   
  
### <a name="zone-scope-level-transfers-in-a-dns-primary-secondary-deployment"></a>傳輸中的 DNS 主要-次要部署的區域範圍層級

流量管理方案需要額外的步驟，來分割至多個不同的區域範圍的區域。 基於這個原因，則需要在區域範圍內將資料傳送到次要伺服器，並將原則和 DNS 用戶端子網路傳輸到次要伺服器進行額外步驟。   
  
設定您的 DNS 基礎結構的主要和次要伺服器之後，傳輸層級的區域範圍會自動執行 dns，使用下列程序。  
  
若要確保區域範圍的層級轉送，DNS 伺服器會使用的 DNS (EDNS0) 選擇 RR 擴充機制。 從範圍區域的所有區域轉送 （AXFR 或 IXFR） 要求都源自 EDNS0 選擇 RR，識別碼的選項預設設定為"65433 」。 如需 EDNSO 的詳細資訊，請參閱 IETF[要求的註解 6891](https://tools.ietf.org/html/rfc6891)。  
  
選擇資源記錄的值是正在傳送要求的區域範圍名稱。 當主要的 DNS 伺服器收到此封包從受信任的次要伺服器時，它可解譯來自該區域範圍的要求。   
  
如果主要伺服器具有該區域範圍回應的傳輸 (XFR) 資料從該範圍。 回應包含具有相同的選項識別碼 」 65433 「 選擇加入的 RR 以及設為相同的區域範圍的值。 次要伺服器會收到這個回應、 從回應中，擷取範圍的資訊和更新特定區域的範圍。  
  
此程序之後，主要伺服器會維護一份受信任的次要複本已傳送通知的這類區域範圍要求。   
  
在區域範圍內任何進一步的更新，如 IXFR 通知會傳送至次要伺服器，使用相同的選擇 RR。 收到該通知的區域範圍會包含該選擇的 RR IXFR request 接著相同的程序如上面所述。  
  
## <a name="how-to-configure-dns-policy-for-primary-secondary-geo-location-based-traffic-management"></a>如何設定主要-次要地理位置流量管理的 DNS 原則

開始之前，請確定您已完成的步驟 > 主題中的所有[地理位置與主要伺服器，根據流量管理的使用 DNS 原則](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md)，且您的主要 DNS 伺服器已設定區域，區域範圍，DNS 用戶端子網路和 DNS 原則。  
  
>[!NOTE]
> 本主題中的指示，將 DNS 用戶端的子網路、 區域範圍和 DNS 原則從主要的 DNS 伺服器複製到次要的 DNS 伺服器是您初始的 DNS 設定和驗證。 在未來可能會想要變更 DNS 用戶端的子網路、 區域範圍，並在主要伺服器上的原則設定。 在此情況下，您可以建立自動化指令碼，以保持與主要伺服器同步處理的次要伺服器。  
  
若要設定主要-次要地理位置基礎查詢回應的 DNS 原則，您必須執行下列步驟。  
  
- [建立次要區域](#create-the-secondary-zones)  
- [設定在主要區域的區域轉送設定](#configure-the-zone-transfer-settings-on-the-primary-zone)  
- [複製 DNS 用戶端的子網路](#copy-the-dns-client-subnets)  
- [次要伺服器上建立的區域範圍](#create-the-zone-scopes-on-the-secondary-server)  
- [設定 DNS 原則](#configure-dns-policy)  
  
下列各節提供詳細的設定指示。  
  
>[!IMPORTANT]
>下列各節包含 Windows PowerShell 命令範例包含許多參數的範例值。 請確定這些命令列中的範例值取代是適用於您的部署，然後再執行這些命令的值。  
><br>中的成員資格**DnsAdmins**，或同等權限，才能執行下列程序。  
  
### <a name="create-the-secondary-zones"></a>建立次要區域

您可以建立您想要複寫至 SecondaryServer1 和 SecondaryServer2 區域的次要複本 （假設指令程式正在執行遠端從單一管理用戶端）。   
  
比方說，您可以建立次要複本的 www.woodgrove.com SecondaryServer1 和 SecondarySesrver2 上。  
  
您可以使用下列 Windows PowerShell 命令來建立次要區域。  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps)。  
  
### <a name="configure-the-zone-transfer-settings-on-the-primary-zone"></a>設定在主要區域的區域轉送設定

您必須設定主要區域設定以便：

1. 允許從主要伺服器的區域轉送到指定的次要伺服器。  
2. 區域更新通知會傳送到次要伺服器的主要伺服器。  
  
您可以使用下列 Windows PowerShell 命令進行區域轉送設定在主要區域。
  
>[!NOTE]
>在下列範例命令中，參數 **-通知**指定主要伺服器會將更新的相關通知傳送至次要複本的 select 清單。  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
如需詳細資訊，請參閱 <<c0> [ 組 DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps)。  
  
  
### <a name="copy-the-dns-client-subnets"></a>複製 DNS 用戶端的子網路

您必須將 DNS 用戶端的子網路從主要伺服器複製次要伺服器。
  
您可以使用下列 Windows PowerShell 命令複製到次要伺服器的子網路。
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="create-the-zone-scopes-on-the-secondary-server"></a>次要伺服器上建立的區域範圍

您必須在次要伺服器上建立的區域範圍。 在 DNS 中的區域範圍也會開始從主要伺服器要求 XFRs。 在主要伺服器上的區域範圍上的任何變更，與包含的區域範圍資訊的通知會傳送到次要伺服器。 然後，次要伺服器就可以使用累加式變更中，來更新其區域範圍。  
  
您可以使用下列 Windows PowerShell 命令，次要伺服器上建立的區域範圍。  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

>[!NOTE]
>在下列範例命令中， **-ErrorAction 忽略**參數都會包括在內，因為每個區域上有的預設區域範圍。 無法建立或刪除預設區域範圍。 管線會嘗試建立該範圍，它將會失敗。 或者，您也可以在兩個的次要區域上建立非預設區域範圍。  
  
如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="configure-dns-policy"></a>設定 DNS 原則

建立子網路之後，資料分割 （區域範圍），而且您已新增記錄，您必須建立連接的子網路和資料分割的原則，以便中 DNS 用戶端子網路的其中一個來源的查詢時，會傳回查詢回應正確的範圍內的區域。 沒有任何原則所需的對應預設區域範圍。  
  
您可以使用下列 Windows PowerShell 命令來建立 DNS 原則，DNS 用戶端的子網路連結和區域範圍。   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
現在使用必要的 DNS 原則，根據地理位置的流量重新導向設定次要 DNS 伺服器。  
  
當 DNS 伺服器收到名稱解析查詢時，DNS 伺服器會評估 DNS 要求，根據設定的 DNS 原則中的欄位。 如果在 名稱解析要求的來源 IP 位址會符合任何原則，相關聯的區域範圍來回應查詢，並將使用者導向的地理位置最接近它們的資源。   
  
您可以建立數以千計的 DNS 原則根據您的流量管理需求，而且所有新的原則都會套用動態-不需要重新啟動 DNS 伺服器-在傳入的查詢。
