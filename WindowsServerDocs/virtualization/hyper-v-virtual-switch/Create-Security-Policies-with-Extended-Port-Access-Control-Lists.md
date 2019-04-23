---
title: 使用延伸連接埠存取控制清單建立安全性原則
description: 本主題提供存取控制清單 (Acl) Windows Server 2016 中的擴充通訊埠的相關資訊。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-hv-switch
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a92e61c3-f7d4-4e42-8575-79d75d05a218
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d847213f0332b57ae38ada444d7a6cd98ab325ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848979"
---
# <a name="create-security-policies-with-extended-port-access-control-lists"></a>使用延伸連接埠存取控制清單建立安全性原則

>適用於：Windows Server （半年通道），Windows Server 2016

本主題提供存取控制清單 (Acl) Windows Server 2016 中的擴充通訊埠的相關資訊。 您可以在 Hyper-V 虛擬交換器上設定延伸 ACL，以允許和封鎖往來的虛擬機器 (VM) 網路流量，而這些虛擬機器是透過虛擬網路介面卡連線到交換器。  
  
本主題涵蓋下列各節。  
  
-   [詳細的 ACL 規則](#bkmk_detailed)  
  
-   [具狀態的 ACL 規則](#bkmk_stateful)  
  
## <a name="bkmk_detailed"></a>詳細的 ACL 規則  
HYPER-V 虛擬交換器延伸 Acl 可讓您建立詳細的規則，您可以套用至個別的 VM 網路介面卡連線至 HYPER-V 虛擬交換器。 能夠建立詳細的規則可讓企業和雲端服務提供者 (Csp) 的多租用戶共用的伺服器環境中解決網路為基礎的安全性威脅。  
  
使用延伸 ACL，就不再需要建立更廣泛的規則來封鎖或允許往來 VM 之所有通訊協定的所有流量，您現在可以封鎖或允許在 VM 上執行之個別通訊協定的網路流量。 您可以建立延伸的 ACL 規則在 Windows Server 2016 中包含下列 5 個 tuple 的參數組： 來源 IP 位址、 目的地 IP 位址、 通訊協定、 來源連接埠及目的地連接埠。 此外，每個規則還可指定網路流量方向 (進或出)，以及規則支援的動作 (封鎖或允許流量)。  
  
舉例來說，您可以設定 VM 的連接埠 ACL 以允許連接埠 80 上的所有連入及連出 HTTP 和 HTTPS 流量，同時封鎖所有連接埠上所有其他通訊協定的網路流量。  
  
這個指定租用戶 VM 能或不能接收通訊協定流量的功能可以在您設定安全性原則時提供彈性。  
  
### <a name="configuring-acl-rules-with-windows-powershell"></a>使用 Windows PowerShell 設定 ACL 規則  
若要設定延伸 ACL，您必須使用 Windows PowerShell 命令 **Add-VMNetworkAdapterExtendedAcl**。 這個命令有四種不同的語法，每個語法都有截然不同的用法：  
  
1.  將延伸的 ACL 新增至所有的具名 VM-這由第一個參數-VMName 指定網路介面卡。 語法：  
  
    > [!NOTE]  
    > 如果您想要新增一個網路介面卡，而所有的延伸的 ACL，您可以使用參數指定的網路介面卡-VMNetworkAdapterName。  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMName] <string[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny}  
        [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
2.  將延伸 ACL 新增到特定 VM 上的特定虛擬網路介面卡。 語法：  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMNetworkAdapter] <VMNetworkAdapterBase[]> [-Action]  
        <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound |  
        Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort]  
        <string>] [[-Protocol] <string>] [-Weight] <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID  
        <int>] [-Passthru] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
3.  將延伸 ACL 新增到保留給 Hyper-V 主機管理作業系統使用的所有虛擬網路介面卡。  
  
    > [!NOTE]  
    > 如果您想要新增一個網路介面卡，而所有的延伸的 ACL，您可以使用參數指定的網路介面卡-VMNetworkAdapterName。  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction]  
        <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress]  
        <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight] <int> -ManagementOS  
        [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName <string>]  
        [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
4.  將延伸的 ACL 新增至 VM 物件，您必須建立在 Windows PowerShell 中，例如 **$vm = 取得 vm"my_vm"**。 您可以在下一行程式碼中執行此命令來建立延伸 ACL，語法如下：  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VM] <VirtualMachine[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow |  
        Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
### <a name="detailed-acl-rule-examples"></a>詳細的 ACL 規則範例  
下面幾個範例說明如何使用 **Add-VMNetworkAdapterExtendedAcl** 命令來設定延伸連接埠 ACL，並建立 VM 的安全性原則。  
  
-   [強制執行應用程式層級安全性](#bkmk_enforce)  
  
-   [強制執行使用者層級和應用程式層級安全性](#bkmk_both)  
  
-   [提供非 TCP/UDP 應用程式的安全性支援](#bkmk_tcp)  
  
> [!NOTE]  
> 下表中規則參數 **Direction** 的值是基於您在建立規則時，VM 是輸出還是輸入流量。 如果 VM 正在接收流量，則流量是輸入；如果 VM 正在傳送流量，則流量是輸出。 舉例來說，如果您將規則套用到 VM 以封鎖輸入流量，輸入流量的方向是從外部資源傳到 VM。 如果您套用的是封鎖輸出流量的規則，輸出流量的方向是從本機 VM 傳到外部資源。  
  
### <a name="bkmk_enforce"></a>強制執行應用程式層級安全性  
由於許多應用程式伺服器使用標準化的 TCP/UDP 連接埠與用戶端電腦通訊，因此建立封鎖或允許存取應用程式伺服器的規則較為簡單，只要篩選指定給應用程式之連接埠所傳送和接收的流量。  
  
例如，您可能想允許使用者使用遠端桌面連線 (RDP) 登入資料中心的應用程式伺服器。 因為 RDP 使用 TCP 連接埠 3389，您可以快速設定下列規則：  
  
|來源 IP|目的地 IP|通訊協定|來源連接埠|目的地連接埠|Direction|動作|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|TCP|*|3389|向內|允許|  
  
下列兩個範例說明如何使用 Windows PowerShell 命令建立規則。 第一個範例規則會封鎖所有流量到 VM 名為"ApplicationServer"。 第二個範例規則，套用至名為"ApplicationServer"之 VM 的網路介面卡，只允許輸入的 RDP 流量到 VM。  
  
> [!NOTE]  
> 當您建立規則時，您可以使用 **-權數**參數，來判斷在其中為 HYPER-V 虛擬交換器處理規則的順序。 值 **-權數**會以整數表示; 整數較小的規則優先處理較大的規則。 例如，如果您在 VM 網路介面卡套用兩個規則，一個的權數是 1，而另一個的權數是 10，則會優先套用權數是 10 的規則。  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Inbound" -LocalPort 3389 -Protocol "TCP" -Weight 10  
```  
  
### <a name="bkmk_both"></a>強制執行使用者層級和應用程式層級安全性  
因為一個規則可比對 5 個 Tuple IP 封包 (來源 IP、目的地 IP、通訊協定、來源連接埠及目的地連接埠)，所以規則可強制執行比連接埠 ACL 更詳細的安全性原則。  
  
比方說，如果您想要 DHCP 服務提供給有限的用戶端使用一組特定的 DHCP 伺服器的電腦，您就可以在執行 HYPER-V，其中裝載使用者 Vm 之 Windows Server 2016 電腦上設定下列規則：  
  
|來源 IP|目的地 IP|通訊協定|來源連接埠|目的地連接埠|Direction|動作|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|255.255.255.255|UDP|*|67|向外|允許|  
|*|10.175.124.0/25|UDP|*|67|向外|允許|  
|10.175.124.0/25|*|UDP|*|68|向內|允許|  
  
下列範例說明如何使用 Windows PowerShell 命令建立這些規則。  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 255.255.255.255 -RemotePort 67 -Protocol "UDP"-Weight 10  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 67 -Protocol "UDP"-Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 68 -Protocol "UDP"-Weight 20  
```  
  
### <a name="bkmk_tcp"></a>提供非 TCP/UDP 應用程式的安全性支援  
雖然資料中心的多數網路流量為 TCP 和 UDP，仍會有部分流量使用其他通訊協定。 例如，如果您要允許一組伺服器執行仰賴網際網路群組管理通訊協定 (IGMP) 的 IP 多點傳送應用程式，則可以建立下列規則。  
  
> [!NOTE]  
> IGMP 有一個指定的 IP 通訊協定號碼 0x02。  
  
|來源 IP|目的地 IP|通訊協定|來源連接埠|目的地連接埠|Direction|動作|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|0x02|*|*|向內|允許|  
|*|*|0x02|*|*|向外|允許|  
  
下列範例說明如何使用 Windows PowerShell 命令建立這些規則。  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -Protocol 2 -Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -Protocol 2 -Weight 20  
```  
  
## <a name="bkmk_stateful"></a>具狀態的 ACL 規則  
延伸 ACL 的另一個新功能可讓您設定可設定狀態的規則。 具狀態的規則來篩選封包根據 5 個中的屬性封包-來源 IP、 目的地 IP、 通訊協定、 來源連接埠及目的地連接埠。  
  
可設定狀態的規則有下列功能：  
  
-   一律允許流量且不會用來封鎖流量。  
  
-   如果您將參數 **Direction** 的值指定為輸入，而且流量符合規則，則 Hyper-V 虛擬交換器會動態建立相符的規則，允許 VM 傳送輸出流量以回應外部資源。  
  
-   如果您將參數 **Direction** 的值指定為輸出，而且流量符合規則，則 Hyper-V 虛擬交換器會動態建立相符的規則，允許外部資源輸入流量以便讓 VM 接收。  
  
-   這些規則包含以秒測量的逾時屬性。 當網路封包送達交換器且封包符合可設定狀態的規則時，Hyper-V 虛擬交換器會建立一個狀態，允許相同流量且任一方向的所有後續封包。 如果在逾時值指定的時間內任一方向都沒有流量，則該狀態就會到期。  
  
下列範例說明如何使用可設定狀態的規則。  
  
### <a name="allow-inbound-remote-server-traffic-only-after-it-is-contacted-by-the-local-server"></a>只在本機伺服器連絡到遠端伺服器時才允許輸入流量  
在某些情況下必須採用可設定狀態的規則，因為只有可設定狀態的規則可以追蹤建立的已知連線，並將其與來自其他連線的連線做區分。  
  
例如，如果您要讓 VM 應用程式伺服器在連接埠 80 起始連到網際網路上 Web 服務的連線，而且希望遠端網頁伺服器能夠回應 VM 流量，則可以設定可設定狀態的規則，允許從 VM 到 Web 服務的初始輸出流量；由於規則可以設定狀態，也可以允許流量從網頁伺服器流回 VM。 基於安全性理由，您可以封鎖所有其他傳到 VM 的輸入網路流量。  
  
為了完成這個規則設定，您可以使用下表的設定。  
  
> [!NOTE]  
> 由於排版限制以及下表的資訊量，資訊顯示的方式會與本文件之前的表格有些差異。  
  
|參數|規則 1|規則 2|規則 3|  
|-------------|----------|----------|----------|  
|來源 IP|*|*|*|  
|目的地 IP|*|*|*|  
|通訊協定|*|*|TCP|  
|來源連接埠|*|*|*|  
|目的地連接埠|*|*|80|  
|Direction|向內|向外|向外|  
|動作|拒絕|拒絕|允許|  
|可設定狀態|否|否|是|  
|逾時 (秒)|N/A|N/A|3600|  
  
可設定狀態的規則允許 VM 應用程式伺服器連線到遠端網頁伺服器。 第一個封包傳出之後，Hyper-V 虛擬交換器會動態建立兩個流量狀態，以允許所有從遠端網頁伺服器傳出及傳回的封包。 如果伺服器間的封包流量停止，流量狀態會在指定的逾時值 3600 秒或一個小時逾時。  
  
下列範例說明如何使用 Windows PowerShell 命令建立這些規則。  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1   
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Outbound" 80 "TCP" -Weight 100 -Stateful -Timeout 3600  
```  
  


