---
title: 使用 DNS 原則 Split-Brain DNS 部署
description: 本主題是 DNS 原則案例指南適用於 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b25ac752ea347f4d184628eb26bc7e297443306
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>使用 DNS Split\ 蛋 DNS 部署原則

>適用於：Windows Server 2016

您可以使用本主題以了解如何在 Windows Server 設定 DNS 原則&reg;對於 split-brain DNS 部署，2016年有兩個版本的單一區域-一個內部使用者在您的組織企業網路，另一個外部使用者，通常是在網際網路上的使用者。

>[!NOTE]
>如何使用 DNS 原則 split\ 蛋 DNS Active Directory 部署整合 DNS 區域中的資訊，請查看[使用 DNS 原則 Split-Brain DNS Active Directory 中的](dns-sb-with-ad.md)。

之前，此案例，您必須 DNS 系統管理員，維護兩個不同的 DNS 伺服器，每個設定的使用者，內外每個提供服務。 如果只有數區域中的資料可讓您已 split\ brained 或兩個區域 （內外） 已委派給相同的父系網域，這會成為管理問題。 

另一個 split-brain 部署的組態案例 DNS 名稱解析是選擇性遞迴控制項。 有時中的企業 DNS 伺服器都必須執行遞迴解析度內部的使用者，在網際網路上，同時也必須做為外部使用者的授權名稱伺服器，並封鎖遞迴它們。 

本主題包含下列各節。

- [DNS Split-Brain 部署的範例](#bkmk_sbexample)
- [DNS 選擇性遞迴控制項的範例](#bkmk_recursion)

##<a name="bkmk_sbexample"></a>DNS Split-Brain 部署的範例
以下是您可以如何使用 DNS 原則來完成之前所述的 split-brain DNS 案例的範例。

本節下列主題。

- [DNS Split-Brain 部署的運作方式](#bkmk_sbhow)
- [如何設定 DNS Split-Brain 部署](#bkmk_sbconfigure)


此範例中使用虛構公司，以 Contoso，維持在 www.career.contoso.com 更上層樓網站。

網站有兩個版本，一個用於內部使用者內部職位何處可使用。 在本機的 IP 位址 10.0.0.39 使用此內部網站。 

第二個版本的相同的網站，可在公用 IP 位址 65.55.39.10 公用版本。

DNS 原則不存在，系統管理員，才能主機上不同的 Windows Server DNS 伺服器下列兩個區域和管理另行購買。 

使用 DNS 原則這些區域可以立即裝載相同的 DNS 伺服器上。  

下圖描述此案例。

![Split-Brain DNS 部署](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


##<a name="bkmk_sbhow"></a>DNS Split-Brain 部署的運作方式

時所需的 DNS 原則設定的 DNS 伺服器，每個名稱解析要求被評估 DNS 伺服器上的原則。

伺服器介面用於在此範例中為準則內外戶端來區分公司。

如果時，收到查詢伺服器介面比對任何原則，相關的區域範圍用來查詢回應。 

因此，在我們的範例，私人 IP (10.0.0.56) 上接收 www.career.contoso.com DNS 查詢收到 DNS 回應包含內部 IP 位址。和在公用網路介面收到 DNS 查詢接收 DNS 回應包含 （這是正常查詢解析度一樣） 的區域預設範圍的公用 IP 位址。  

##<a name="bkmk_sbconfigure"></a>如何設定 DNS Split-Brain 部署
若要使用 DNS 原則設定 DNS Split-Brain 部署，您必須使用下列步驟。

- [建立區域範圍](#bkmk_zscopes)  
- [若要的區域領域加入資料](#bkmk_records)  
- [建立 DNS 原則](#bkmk_policies)

下列章節提供詳細的設定指示操作。

>[!IMPORTANT]
>以下的各節包含包含許多參數值範例範例 Windows PowerShell 命令。 請確認值是適用於您的部署，執行下列命令之前，先取代範例值這些命令列中。 

###<a name="bkmk_zscopes"></a>建立區域範圍

時區領域是區域的唯一執行個體。 DNS 區域可以有多個區域領域，與每個包含 DNS 記錄它自己設定的區域範圍。 相同記錄可能會出現在多個領域，以不同的 IP 位址或相同的 IP 位址。 

>[!NOTE]
>根據預設，區域領域存在於 DNS 區域。 這個區域領域作為區域，具有相同的名稱，並在這個領域中工作舊版 DNS 作業。 這個預設區域的範圍會主機外部 www.career.contoso.com 的版本。

您可以使用下列命令範例分割建立內部區域領域區域範圍 contoso.com。 內部區域範圍會用來保留內部 www.career.contoso.com 的版本。

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

如需詳細資訊，請查看[新增-DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records"></a>若要的區域領域加入資料

下一個步驟是加入 （的外部戶端） 代表預設與 Web 伺服器主機的兩個區域領域-內部到的資料。 

在內部區域範圍，記錄**www.career.contoso.com**使用的 IP 位址 10.0.0.39，也就是私人 IP; 中新增了在 [預設區域領域相同記錄， **www.career.contoso.com**，使用的 IP 位址 65.55.39.10 中新增了。

不**– ZoneScope**記錄被新增到區域預設範圍時範例下列命令中提供的參數。 這是類似記錄加入香草區域。

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

如需詳細資訊，請查看[新增-DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx)。

###<a name="bkmk_policies"></a>建立 DNS 原則

伺服器介面外部網路和連絡找出您所建立的時區範圍之後，您必須建立連接內外區域領域 DNS 原則。

>[!NOTE]
>此範例中為準則使用伺服器介面，來區分內外戶端。 區分外部和內部另一個方法是使用 client 子網路為條件。 如果您找出內部戶端所屬的子網路，您可以設定來區分根據 client 子網路的 DNS 原則。 如何設定流量管理使用 client 子網路條件資訊，請查看[使用 DNS 原則主要伺服器的地理位置型流量管理的](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers)。

當 DNS 伺服器接收私人介面在查詢時，從內部區域範圍傳回 DNS 查詢回應。

>[!NOTE]
>不原則所需的對應區域預設範圍。 

在下列範例命令中，10.0.0.56 是 IP 位址的私人網路介面上，在上圖中所示。

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

如需詳細資訊，請查看[新增-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。  


## <a name="bkmk_recursion"></a>DNS 選擇性遞迴控制項的範例

以下是您可以如何使用 DNS 原則來完成之前所述的 DNS 選擇性遞迴控制案例的範例。

本節下列主題。

- [如何 DNS 選擇性遞迴控制運作](#bkmk_recursionhow)
- [如何設定 DNS 選擇性遞迴控制項](#bkmk_recursionconfigure)

此範例中使用與先前的範例，以 Contoso，維持在 www.career.contoso.com 更上層樓網站相同虛構公司。

DNS split-brain 部署範例中，回應外部和內部戶端相同的 DNS 伺服器，並提供不同的答案。 

某些 DNS 部署可能需要執行遞迴名稱解析為除了作為外部戶端授權名稱伺服器內部戶端相同的 DNS 伺服器。 這個情況稱為 DNS 選擇性遞迴控制項。

在舊版的 Windows Server、 讓遞迴適用於對短片它已支援的所有區域整個 DNS 伺服器上。 因為外部查詢也聆聽 DNS 伺服器，遞迴被支援和外部戶端，開放解析程式進行的 DNS 伺服器。 

設定開放解析可能受到資源耗盡，可能會被濫用惡意用來建立反映攻擊 DNS 伺服器。 

因此，Contoso DNS 系統管理員不想 contoso.com 的外部戶端執行遞迴名稱解析為的 DNS 伺服器。 有時，只需要遞迴內部戶端，控制項遞迴控制可能會封鎖外部戶端。 

下圖描述此案例。

![選擇遞迴控制項](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>如何 DNS 選擇性遞迴控制運作

如果您收到 Contoso DNS 伺服器是未經授權的查詢，如的 www.microsoft.com，然後名稱解析要求評估 DNS 伺服器上的原則。 

這些查詢未落在任何時區，因為區域層級原則 \ 不評估 （如 split-brain example\ 中所定義）。 

DNS 伺服器評估遞迴原則及接收到的私人介面相符查詢**SplitBrainRecursionPolicy**。 這項原則指向尚未遞迴遞迴範圍。

DNS 伺服器執行從網際網路、 www.microsoft.com 取得解答遞迴，然後回應本機快取。 

如果查詢收到外部介面、 DNS 原則找不到，以及預設遞迴設定-這是**停用**-套用。

如此可防止 server 作為的外部戶端，開放解析時它做為 [快取的內部戶端器。 

### <a name="bkmk_recursionconfigure"></a>如何設定 DNS 選擇性遞迴控制項

若要使用 DNS 原則設定 DNS 選擇性遞迴控制項，您必須使用下列步驟。

- [建立 DNS 遞迴範圍](#bkmk_recscopes)
- [建立 DNS 遞迴原則](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>建立 DNS 遞迴範圍

遞迴範圍是唯一的執行個體群組的控制遞迴 DNS 伺服器上的設定。 遞迴範圍包含轉送程式的清單，並指定遞迴是否已支援。 DNS 伺服器可以有許多遞迴範圍。 

舊版遞迴設定和轉送程式清單稱為遞迴預設範圍。 您無法新增或移除預設遞迴範圍由名稱點 \("."\).

在此範例中，預設遞迴已停用，遞迴功能的位置建立新遞迴領域內部戶端時。

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

如需詳細資訊，請查看[新增-DnsServerRecursionScope](https://technet.microsoft.com/library/mt126268.aspx)

#### <a name="bkmk_recpolicy"></a>建立 DNS 遞迴原則

您可以選擇一組查詢符合的條件特定的遞迴範圍遞迴原則建立 DNS 伺服器。 

如果您的 DNS 伺服器不適用於某些查詢、 DNS 伺服器遞迴原則可讓您控制如何解析查詢。 

在此範例中，內部遞迴遞迴支援的範圍是相關聯的私人網路介面

您可以使用下列命令範例設定 DNS 遞迴原則。

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.39"
    

如需詳細資訊，請查看[新增-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。

現在 DNS 伺服器設定選擇性遞迴控制內部戶端支援是以所需的 DNS 原則 split-brain 名稱伺服器或 DNS 伺服器。

您可以建立數千 DNS 原則根據您的資料傳輸管理的需求，且所有的新原則已經套用動態-不需要重新 DNS 伺服器-連入查詢。 

如需詳細資訊，請查看[DNS 原則案例指南](DNS-Policy-Scenario-Guide.md)。
