---
title: 地理位置型主要次要部署的流量管理，使用 DNS 原則
description: 本主題是 DNS 原則案例指南適用於 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4c78a0198e29fb59f30fd8ad776c7f200312d014
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>地理位置型主要次要部署的流量管理，使用 DNS 原則

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何建立的地理位置資料傳輸管理 DNS 原則，當您的 DNS 部署包含主要和次要 DNS 伺服器。  

上述案例中，[使用 DNS 原則主要伺服器的地理位置型流量管理的](primary-geo-location.md)，提供的地理位置資料傳輸管理 DNS 原則設定的主要 DNS 伺服器上的指示操作。 網際網路基礎結構，但的 DNS 伺服器的廣泛地部署中所在的區域寫入複本儲存選取且安全的主要伺服器上與唯讀複本區域保留多個次要伺服器上的主要次要模型。   
  
第二個伺服器使用授權傳輸 (AXFR) 和增量區域傳輸 (IXFR) 區域傳輸通訊協定要求，及接收區域的更新，包括新的主要 DNS 伺服器上的區域的變更。   
  
>[!NOTE]
>如需 AXFR，查看網際網路工程設計工作推動 (IETF)[要求意見 5936 的](https://tools.ietf.org/rfc/rfc5936.txt)。 如需 IXFR，查看網際網路工程設計工作推動 (IETF)[要求意見 1995 年的](https://tools.ietf.org/html/rfc1995)。  
  
## <a name="bkmk_example"></a>主要次要地理位置型流量管理範例  
以下是如何使用 DNS 原則中的主要次要部署達成流量重新導向以執行 DNS 查詢 client 的所在位置為基礎的範例。  
  
此範例中使用兩個虛構公司-Contoso 雲端服務，提供網頁和網域裝載方案。及 Woodgrove 食物服務提供多個城市的食物傳送服務全球有名 woodgrove.com 的網站。  
  
為了確保 woodgrove.com 針對回應式體驗從他們的網站，Woodgrove 想要歐洲戶端導向歐洲 datacenter 美國戶端導向美國資料中心。 針對其他地方找到世界可以導向的資料中心。  
  
Contoso 雲端服務將有兩個 datacenter 一個美國和歐洲時，以 Contoso 主控訂購 woodgrove.com 入口網站其食物另一個。  
  
Contoso DNS 部署包括有兩個次要伺服器：**SecondaryServer1**，以 10.0.0.2; 的 IP 位址以及**SecondaryServer2**，以 10.0.0.3 的 IP 位址。 這些次要伺服器為 SecondaryServer1 位於歐洲和 SecondaryServer2 位於美國與做為名稱伺服器兩個不同的地區，
  
還有主要寫入區域複本，在**PrimaryServer**（IP 位址 10.0.0.1），其中區域的變更。 次要伺服器區規則傳輸、次要伺服器都隨時取得最新的時區 PrimaryServer 在任何新的變更。
  
下圖描述此案例。
  
![主要次要地理位置型流量管理範例](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="bkmk_works"></a>主要次要 DNS 系統的運作方式

當您要部署的主要次要 DNS 部署地理位置資料傳輸管理時，請務必以了解如何在一般的主要次要區域學習區域範圍層級傳輸之前發生的傳輸。 下列章節區域區域範圍層級轉送上提供的資訊。  
  
- [在 [主要次要 DNS 部署區域轉送](#bkmk_zone)  
- [在 [主要次要 DNS 部署區域範圍層級轉送](#bkmk_scope)  
  
### <a name="bkmk_zone"></a>在 [主要次要 DNS 部署區域轉送

您可以建立主要次要 DNS 部署，並進行下列步驟同步區域。  
1. 當您安裝 DNS 時，主要 DNS 伺服器上建立主要區域。  
2. 在次要伺服器，建立的區域，指定的主要伺服器。   
3. 主要的伺服器，您可以新增第二個伺服器上主要區域信任次要連結。   
4. 次要區域完整區域傳輸要求 (AXFR)，以及取得區域的複本。   
5. 需要時，主要伺服器會傳送通知區域的更新相關的第二個伺服器。  
6. 次要伺服器進行增量區域傳輸要求 (IXFR)。 因此，次要伺服器維持與主要伺服器同步。   
  
### <a name="bkmk_scope"></a>在 [主要次要 DNS 部署區域範圍層級轉送

交通管理案例需要額外的步驟來區域的不同領域插入磁碟分割區。 因為額外的步驟會需要區域領域中的資料傳輸到的第二個伺服器，並傳送原則和 DNS Client 子網路的第二個伺服器。   
  
設定伺服器主要和次要 DNS 基礎結構之後，區域範圍層級轉送會自動執行，DNS，使用下列程序。  
  
若要確保區域範圍層級轉送，DNS 伺服器使用 DNS (EDNS0) 加入 RR 擴充機制。 EDNS0 加入 RR，其選項 ID 預設設定為「65433」是源自範圍的區域的所有區域（AXFR 或是 IXFR）傳送要求。 如需 EDNSO，查看 IETF[要求意見 6891 的](https://tools.ietf.org/html/rfc6891)。  
  
選擇 RR 值是區域範圍名稱正在傳送要求。 當主要的 DNS 伺服器這封包收到來自信任的第二個伺服器時，它會轉譯為即將區域領域要求。   
  
如果主要伺服器區領域回應的資料傳輸（xfr 跑車）該範圍的。 回應包含選擇加入以相同的選項 ID」65433」的 RR 和值設定為相同的時區範圍。 次要伺服器接收此回應、從所做出的回應擷取的範圍資訊和更新特定範圍的區域。  
  
此程序後主要伺服器維護的受信任的次要已傳送通知的此類區域範圍要求的連結。   
  
適用於所有區域領域中的進一步更新，IXFR 通知的第二個伺服器，使用相同的加入 RR。 接收的通知區域領域可包含該加入 RR IXFR 要求和相同的程序，如上文所述如下。  
  
## <a name="bkmk_config"></a>如何設定主要次要地理位置型流量管理 DNS 原則

在您開始之前，請確定您已完成此主題中的步驟執行的所有[使用 DNS 原則主要伺服器的地理位置型流量管理](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md)，並與區域，區域範圍、DNS Client 子網路和 DNS 原則設定您的主要 DNS 伺服器。  
  
>[!NOTE]
> 本主題中的指示來次要 dns 複製 DNS Client 子網路，區域的範圍，以及 DNS 原則的主要的 DNS 伺服器的驗證和初始 DNS 設定。 未來可能會想要變更 DNS Client 子網路、區域的範圍，以及主要伺服器原則設定。 在這種情況下，您可以建立保持同步的主要伺服器次要伺服器自動化的指令碼。  
  
若要設定為主要次要地理位置型查詢回應 DNS 原則，您必須執行下列步驟。  
  
- [建立次要區域](#bkmk_secondary)  
- [時區傳輸上設定「主要」區域](#bkmk_zonexfer)  
- [複製 DNS Client 子網路](#bkmk_client)  
- [第二個伺服器上建立的時區領域](#bkmk_zonescopes)  
- [設定 DNS 原則](#bkmk_dnspolicy)  
  
下列章節提供詳細的設定指示操作。  
  
>[!IMPORTANT]
>以下的各節包含包含許多參數值範例範例 Windows PowerShell 命令。 請確認值是適用於您的部署，執行下列命令之前，先取代範例值這些命令列中。  
><br>資格在**DnsAdmins**，或等，才能執行下列程序。  
  
### <a name="bkmk_secondary"></a>建立次要區域

您可以建立第二份您想要複寫 SecondaryServer1 和 SecondaryServer2 區域（假設 cmdlet 執行遠端從單一管理 client）。   
  
例如，您可以建立 www.woodgrove.com 第二份 SecondaryServer1 SecondarySesrver2 上。  
  
您可以使用下列的 Windows PowerShell 命令來建立次要區域。  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

如需詳細資訊，請查看[新增-DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps)。  
  
### <a name="bkmk_zonexfer"></a>時區傳輸上設定「主要」區域

您必須設定主要區域，：

1. 允許從主要伺服器次要指定的伺服器區傳輸。  
2. 區域更新通知主要的伺服器來傳送到的第二個伺服器。  
  
您可以使用下列的 Windows PowerShell 命令主要區域上設定的區域傳輸設定。
  
>[!NOTE]
>下列範例命令的參數在**-通知**指定主要伺服器傳送更新通知來選取清單中次要連結。  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
如需詳細資訊，請查看[設定為 DnsServerPrimaryZone](https://https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps)。  
  
  
### <a name="bkmk_client"></a>複製 DNS Client 子網路

您必須將 DNS Client 子網路主要伺服器複製的第二個伺服器。
  
您可以使用下列的 Windows PowerShell 命令的第二個伺服器複製子網路。
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

如需詳細資訊，請查看[新增-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="bkmk_zonescopes"></a>第二個伺服器上建立的時區領域

您必須建立區域領域次要伺服器上。 在 DNS 區域領域也會開始要求 XFRs 主要伺服器。 主要伺服器上的時區領域上的任何變更，以通知包含區域範圍資訊傳送至次要伺服器。 第二個伺服器可以再更新增量變更其時區範圍。  
  
您可以使用下列 Windows PowerShell 命令建立的時區領域次要伺服器上。  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

>[!NOTE]
>在這些範例命令中，**-ErrorAction 忽略]**參數預設區域領域存在於每個區域因為是包含。 預設區域領域無法建立復原或者。 管線會導致嘗試建立該範圍，它將會失敗。 或者，您可以在兩個次要區域建立區域非預設範圍。  
  
如需詳細資訊，請查看[新增-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="bkmk_dnspolicy"></a>設定 DNS 原則

子網路建立後的磁碟分割（區域領域），而且您已新增記錄、查詢回應 DNS client 子網路的來源查詢時，會傳回正確的範圍的區域的您必須建立連接子網路和的磁碟分割的原則。 不原則所需的對應區域預設範圍。  
  
您可以使用下列的 Windows PowerShell 命令來建立 DNS 原則連結 DNS Client 子網路，以及區域範圍。   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

如需詳細資訊，請查看[新增-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
現在的設定所需的 DNS 原則，將根據地理位置資料傳輸次要 DNS 伺服器。  
  
當 DNS 伺服器接收名稱解析查詢時、DNS 伺服器評估 DNS 要求針對 DNS 原則設定中的欄位。 如果名稱解析要求來源 IP 位址比對任何原則，相關的區域範圍用來回應查詢，和使用者導向它們地理位置最接近的資源。   
  
您可以建立數千 DNS 原則根據您的資料傳輸管理的需求，且所有的新原則已經套用動態-不需要重新 DNS 伺服器-連入查詢。
