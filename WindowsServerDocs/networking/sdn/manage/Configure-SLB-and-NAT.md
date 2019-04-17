---
title: 設定的軟體負載平衡器負載平衡和網路位址轉譯 (NAT)
description: 本主題是輔助的軟體定義網路上如何管理承租人工作負載和 Windows Server 2016 Virtual 網路的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f0393db564061caa0bc8f18b1d623f24749b46c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>設定的軟體負載平衡器負載平衡和網路位址轉譯 (NAT)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何使用軟體定義網路 \(SDN\) 軟體負載平衡器 \(SLB\) 提供輸出網路位址轉譯 NAT，輸入 NAT，或負載平衡之間多個應用程式執行個體。

本主題包含下列各節。

- [軟體負載平衡器概觀](#bkmk_slbover)
- [範例：建立負載平衡 virtual 網路上的兩個 Vm 的集區與公用 VIP](#bkmk_publicvip)
- [範例：使用 SLB 輸出 NAT](#bkmk_obnat)
- [範例：後端集區中新增網路介面](#bkmk_backend)
- [範例：轉送流量使用軟體負載平衡器](#bkmk_forward)

## <a name="bkmk_slbover"></a>軟體負載平衡器概觀

SDN 軟體負載平衡器 \(SLB\) 提供高可用性和網路效能，您的應用程式。 它是層級 4 \（TCP、UDP\）負載平衡器分散傳入良好的服務執行個體雲端服務或虛擬機器定義負載平衡將流量之。

您可以設定 SLB 執行動作。

* 負載平衡傳入流量外部虛擬機器 \(VMs\) virtual 網路。 這稱為公用 VIP 負載平衡。
* 負載平衡傳入流量之間 Vm 中 virtual 網路、之間 Vm 中的雲端服務，或在跨先 virtual 網路 Vm 先在電腦之間。 
* 往後 VM 的網路流量 virtual 網路外部使用網路位址轉譯 (NAT) 的目的地。  這稱為 nat 輸出。
* 將外部流量轉寄給特定 VM。  這稱為輸入 nat。

>[!IMPORTANT]
>已知的問題會防止負載平衡器中的物件 NetworkController Windows PowerShell 模組在 Windows Server 2016 5 正確運作。 因應措施是改為使用動態 hash 表格和 Invoke-WebRequest。 下列範例圖示範此方法。


## <a name="bkmk_publicvip"></a>範例：建立負載平衡 virtual 網路上的兩個 Vm 的集區與公用 VIP

您可以使用此範例中，以做為可要求 VIP 集區成員建立負載平衡器物件公用 VIP 和兩個 Vm。  此程式碼範例，也新增 HTTP 健康探查偵測您是否有一個集區成員變成非回應。

###<a name="step-1-prepare-the-load-balancer-object"></a>步驟 1：準備負載平衡器物件
您可以使用以下的範例準備負載平衡器物件。

    $lbresourceId = "LB2"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

###<a name="step-2-assign-a-front-end-ip"></a>步驟 2：指派前端 IP
前端 IP 通常為 Virtual IP (VIP)。  必須從未使用的 IP 其中一個 IP 集區之前贈與負載平衡器管理員邏輯網路獲得 VIP。

您可以使用以下的範例指派前端 IP 位址。

    $vipip = "10.127.132.5"
    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

###<a name="step-3-allocate-a-backend-address-pool"></a>步驟 3：配置集區端地址
地址後端集區包含動態 IPs (DIPs) 組成負載平衡集 Vm 的成員。 在此步驟，您只能配置集區。IP 設定會在接下來的步驟來新增。

您可以使用以下的範例配置後端地址集區。
 
    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-4-define-a-health-probe"></a>步驟 4：定義健康探查
健康探查使用負載平衡器來判斷成員後端集區的健康狀態。 使用此範例中，您定義查詢 HTTP 探查 RequestPath 的「日 health.htm」。  查詢執行每個 5 秒，所指定的 IntervalInSeconds 屬性。

健康探查必須接收的健康狀態端 ip 探查 11 連續查詢 200 HTTP 回應程式碼。 如果不良好端 IP，負載平衡器不會傳送資料傳輸至 IP。

>[!Note]
>請務必的任何存取控制清單套用至端 IP 不會阻止或從第一次的 IP 子網路中的資料傳輸因為這是原始的探查點。

您可以使用以下的範例，來定義健康探查。
 
    $lbprobe = @{}
    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$lbresourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties = @{}
    $lbprobe.properties.protocol = "HTTP"
    $lbprobe.properties.port = "80"
    $lbprobe.properties.RequestPath = "/health.htm"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11
    $lbproperties.probes += $lbprobe

###<a name="step-5-define-a-load-balancing-rule"></a>步驟 5：定義負載平衡規則
此負載平衡規則定義流量之到達前端 IP 是傳送到端 IP。  在此範例中，TCP 連接埠 80 流量會傳送至端集區。

您可以使用以下的範例，來定義負載平衡規則。

    $lbrule = @{}
    $lbrule.ResourceId = "webserver1"
    $lbrule.properties = @{}
    $lbrule.properties.FrontEndIPConfigurations = @()
    $lbrule.properties.FrontEndIPConfigurations += $fe
    $lbrule.properties.backendaddresspool = $backend 
    $lbrule.properties.protocol = "TCP"
    $lbrule.properties.frontendPort = 80
    $lbrule.properties.Probe = $lbprobe
    $lbproperties.loadbalancingRules += $lbrule

###<a name="step-6-add-the-load-balancer-configuration-to-network-controller"></a>步驟 6：加入 Network Controller 負載平衡器設定
到目前為止在此範例中，所有建立的物件的記憶體中的 Windows PowerShell 工作階段。 這個步驟加入 Network Controller 的物件。

您可以使用以下的範例加入 Network Controller 負載平衡器設定。

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

在此步驟後，您必須遵循下列到此端集區中新增的網路介面範例。

## <a name="bkmk_obnat"></a>範例：使用 SLB 輸出 NAT

您可以使用此範例中，設定 SLB 後端集區提供輸出 NAT 功能連接到網際網路輸出 vm virtual 網路位址私密空間。

###<a name="step-1-create-the-loadbalancer-properties-front-end-ip-and-backend-pool"></a>步驟 1：建立 loadbalancer 屬性，前端 IP 和端集區。
若要建立 loadbalancer 屬性，前端 IP 和端集區，您可以使用以下的範例。

    $lbresourceId = "OutboundNATMembers"
    $vipip = "10.127.132.7"

    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-2-define-the-outbound-nat-rule"></a>步驟 2：定義輸出 NAT 規則
您可以使用以下的範例定義的輸出 NAT 規則。 

    $onat = @{}
    $onat.ResourceId = "onat1"
    $onat.properties = @{}
    $onat.properties.frontendipconfigurations = @()
    $onat.properties.frontendipconfigurations += $fe
    $onat.properties.backendaddresspool = $backend
    $onat.properties.protocol = "ALL"
    $lbproperties.OutboundNatRules += $onat

###<a name="step-3-add-the-load-balancer-object-in-network-controller"></a>步驟 3: Network Controller 中新增負載平衡器物件
您可以使用以下的範例 Network Controller 中新增負載平衡器物件。

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

在下一個步驟中，您可以新增您想要提供網際網路存取權的網路介面。

## <a name="bkmk_backend"></a>範例：後端集區中新增網路介面
您可以使用此範例中新增網路介面後端集區。

您必須重複此步驟針對每個可處理網路介面要求 VIP 對的。 您也可以重複此程序在單一網路介面新增多個負載平衡器物件。 例如，如果您的網頁伺服器 VIP 負載平衡器物件不同負載平衡器物件，以提供 nat 輸出。
    
### <a name="step-1-get-the-load-balancer-object-containing-the-back-end-pool-to-which-you-will-add-a-network-interface"></a>步驟 1：取得負載平衡器物件包含後端集區，您將會新增網路介面
您可以使用以下的範例擷取負載平衡器物件。

    $lbresourceid = "LB2"
    $lb = (Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Get" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -DisableKeepAlive -UseBasicParsing).content | convertfrom-json 

### <a name="step-2-get-the-network-interface-and-add-the-backendaddress-pool-to-the-loadbalancerbackendaddresspools-array"></a>步驟 2：取得的網路介面並新增到 loadbalancerbackendaddresspools 陣列 backendaddress 集區。
您可以使用以下的範例取得的網路介面並新增到 loadbalancerbackendaddresspools 陣列 backendaddress 集區。

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    
### <a name="step-3-put-the-network-interface-to-apply-the-change"></a>步驟 3：放套用變更的網路介面
您可以使用以下的範例將網路介面套用變更。

    new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force
 
## <a name="bkmk_forward"></a>範例：轉送流量使用軟體負載平衡器
如果您需要不定義個人連接埠對應 virtual 網路上的單一網路介面 Virtual IP，您可以建立 L3 轉接規則。  此規則會轉送透過指派 VIP，必須包含在 PublicIPAddress 物件 VM 中的所有的資料傳輸。

如果 VIP 和 DIP 定義相同的子網路，然後這相當於執行而不需要 NAT L3 轉接

>[!NOTE]
>此程序不需要建立負載平衡器物件。  指定的網路介面 PublicIPAddress 是不足，無法軟體負載平衡器執行設定的資訊。

###<a name="step-1-create-a-public-ip-object-to-contain-the-vip"></a>步驟 1：建立包含 VIP 公用 IP 物件
您可以使用以下的範例建立公用 IP 物件。

    $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
    $publicIPProperties.ipaddress = "10.127.132.6"
    $publicIPProperties.PublicIPAllocationMethod = "static"
    $publicIPProperties.IdleTimeoutInMinutes = 4
    $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri

####<a name="step-2-assign-the-publicipaddress-to-a-network-interface"></a>步驟 2: PublicIPAddress 為網路介面
您可以使用以下的範例 PublicIPAddress 為網路介面。

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
    New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties



 