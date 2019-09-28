---
title: 網路控制卡安全性
description: 您可以使用本主題來瞭解如何為網路控制站與其他軟體和裝置之間的所有通訊設定安全性。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: bd44b4d696fef3c167c7bcd4ffbc7ca79009cebc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405974"
---
# <a name="secure-the-network-controller"></a>保護網路控制卡

在本主題中，您將瞭解如何為[網路控制](../technologies/network-controller/network-controller.md)站與其他軟體和裝置之間的所有通訊設定安全性。 

您可以保護的通訊路徑包括管理平面上的 Northbound 通訊、叢集中的網路控制站虛擬機器\(vm\)之間的叢集通訊，以及資料上的 Southbound 通訊底板.

1. **Northbound 通訊**。 網路控制站會在管理平面上，\-使用具有 SDN 功能的管理軟體（ \(例如\)Windows PowerShell 和 System Center Virtual Machine Manager SCVMM）進行通訊。 這些管理工具可讓您定義網路原則，並建立網路的目標狀態，讓您可以比較實際的網路設定，使實際的設定與目標狀態保持同位。

2. **網路控制**站叢集通訊。 當您將三個或多個 Vm 設定為網路控制卡叢集節點時，這些節點會彼此通訊。 這項通訊可能與跨節點同步處理和複寫資料，或網路控制站服務之間的特定通訊有關。

3.  **Southbound 通訊**。 網路控制站會使用 SDN 基礎結構和其他裝置（例如軟體負載平衡器、閘道和主機電腦），在資料平面上進行通訊。 您可以使用網路控制站來設定和管理這些 southbound 裝置，讓它們維持您為網路設定的目標狀態。


## <a name="northbound-communication"></a>Northbound 通訊

網路控制站支援用於 Northbound 通訊的驗證、授權和加密。 下列各節提供如何設定這些安全性設定的相關資訊。

### <a name="authentication"></a>驗證

當您設定網路控制站 Northbound 通訊的驗證時，您會允許網路控制站叢集節點和管理用戶端驗證與其通訊之裝置的身分識別。

網路控制卡支援管理用戶端與網路控制站節點之間的下列三種驗證模式。

>[!Note]
>如果您要使用 System Center Virtual Machine Manager 部署網路控制卡，則只支援**Kerberos**模式。

1. **Kerberos**。 將管理用戶端和所有網路控制卡叢集節點聯結到 Active Directory 網域時，請使用 Kerberos 驗證。 Active Directory 網域必須擁有用於驗證的網域帳戶。

2. **X509**。 針對未加入 Active Directory\-網域的管理用戶端，請使用 X509 進行以憑證為基礎的驗證。 您必須將憑證註冊到所有的網路控制站叢集節點和管理用戶端。 此外，所有節點和管理用戶端都必須信任其他人的憑證。

3. **None**： 在測試環境中使用「無」進行測試，因此不建議在生產環境中使用。 當您選擇此模式時，節點和管理用戶端之間不會執行任何驗證。

您可以使用 Windows PowerShell 命令 **[Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** 搭配_ClientAuthentication_參數來設定 Northbound 通訊的驗證模式。 


### <a name="authorization"></a>Authorization

當您設定網路控制站 Northbound 通訊的授權時，您會允許網路控制站叢集節點和管理用戶端確認與其通訊的裝置是受信任的，而且有權參與交流.

針對網路控制卡支援的每個驗證模式使用下列授權方法。

1.  **Kerberos**。 當您使用 Kerberos 驗證方法時，您會在 Active Directory 中建立安全性群組，然後將授權的使用者和電腦新增至群組，以定義授權與網路控制站通訊的使用者和電腦。 您可以使用 **[安裝 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** Windows PowerShell 命令的_ClientSecurityGroup_參數，將網路控制站設定為使用安全性群組進行授權。 安裝網路控制站之後，您可以使用 **[NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** 命令搭配參數 _-ClientSecurityGroup_來變更安全性群組。 如果使用 SCVMM，您必須在部署期間提供安全性群組做為參數。

2.  **X509**。 當您使用 X509 驗證方法時，網路控制卡只會接受來自網路控制卡之憑證指紋的管理用戶端所提出的要求。 您可以使用 **[安裝 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** Windows PowerShell 命令的_ClientCertificateThumbprint_參數來設定這些指紋。 您可以使用 **[NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** 命令，隨時新增其他用戶端指紋。

3.  **None**： 當您選擇此模式時，節點和管理用戶端之間不會執行任何驗證。 在測試環境中使用「無」進行測試，因此不建議在生產環境中使用。 


### <a name="encryption"></a>加密

Northbound 通訊會使用\(安全通訊端層\) SSL，在管理用戶端和網路控制站節點之間建立加密通道。 Northbound 通訊的 SSL 加密包括下列需求：

- 所有網路控制站節點都必須具有相同的憑證，其中包含伺服器驗證和用戶端驗證用途， \(以\)增強金鑰使用 EKU 延伸模組。 

- 管理用戶端用來與網路控制站通訊的 URI 必須是憑證主體名稱。 憑證主體名稱必須包含網路控制站 REST 端點的完整功能變數名稱（FQDN）或 IP 位址。

- 如果網路控制站節點位於不同的子網，其憑證的主體名稱必須與**NetworkController** Windows PowerShell 命令中用於_RestName_參數的值相同。 

- 所有的管理用戶端都必須信任 SSL 憑證。


### <a name="ssl-certificate-enrollment-and-configuration"></a>SSL 憑證註冊和設定

您必須在網路控制卡節點上手動註冊 SSL 憑證。

註冊憑證之後，您可以將網路控制站設定為使用憑證搭配**NetworkController** Windows PowerShell 命令的 **-ServerCertificate**參數。 如果您已安裝網路控制卡，可以隨時使用 NetworkController 命令來更新**設定**。

>[!NOTE]
>如果您使用 SCVMM，您必須將憑證新增為程式庫資源。 如需詳細資訊，請參閱在[VMM 網狀架構中設定 SDN 網路控制](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)站。

## <a name="network-controller-cluster-communication"></a>網路控制站叢集通訊

網路控制站支援用於網路控制站節點間通訊的驗證、授權和加密。 通訊是透過 WCF\)和 TCP [Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(。

您可以使用**安裝 NetworkControllerCluster** Windows PowerShell 命令的**ClusterAuthentication**參數來設定此模式。 

如需詳細資訊，請參閱[Install-NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster)。

### <a name="authentication"></a>驗證

當您設定網路控制站叢集通訊的驗證時，您會允許網路控制卡叢集節點驗證與其通訊的其他節點的身分識別。

網路控制卡支援網路控制站節點之間的下列三種驗證模式。

>[!NOTE]
>如果您使用 SCVMM 部署網路控制卡，則只支援**Kerberos**模式。

1. **Kerberos**。 當所有網路控制站叢集節點都聯結至 Active Directory 網域，並使用用於驗證的網域帳戶時，您可以使用 Kerberos 驗證。

2. **X509**。 X509 是以\-憑證為基礎的驗證。 當網路控制站叢集節點未加入 Active Directory 網域時，您可以使用 X509 驗證。 若要使用 X509，您必須將憑證註冊到所有網路控制站叢集節點，而且所有節點都必須信任憑證。 此外，在每個節點上註冊之憑證的主體名稱必須與節點的 DNS 名稱相同。

3. **None**： 當您選擇此模式時，網路控制卡節點之間不會執行任何驗證。 此模式僅供測試之用，不建議用於生產環境。

### <a name="authorization"></a>Authorization

當您設定網路控制站叢集通訊的授權時，您會允許網路控制卡叢集節點，確認與其通訊的節點是受信任的，而且具有參與通訊的許可權。

針對網路控制卡支援的每個驗證模式，會使用下列授權方法。

1. **Kerberos**。 網路控制站節點只接受來自其他網路控制站電腦帳戶的通訊要求。 當您使用[NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令的**Name**參數部署網路控制站時，可以設定這些帳戶。

2. **X509**。 網路控制站節點只接受來自其他網路控制站電腦帳戶的通訊要求。 當您使用[NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令的**Name**參數部署網路控制站時，可以設定這些帳戶。

3. **None**： 當您選擇此模式時，網路控制卡節點之間不會執行任何授權。 此模式僅供測試之用，不建議用於生產環境。

### <a name="encryption"></a>加密

網路控制站節點之間的通訊會使用 WCF 傳輸層級加密進行加密。 當驗證和授權方法是 Kerberos 或 X509 憑證時，會使用這種形式的加密。 如需詳細資訊，請參閱下列主題。

- [如何：使用 Windows 認證保護服務](https://msdn.microsoft.com/library/ms734673.aspx)
- [如何：使用 x.509 憑證](https://msdn.microsoft.com/library/ms788968.aspx)保護服務的安全。

## <a name="southbound-communication"></a>Southbound 通訊

網路控制站會與不同類型的裝置互動，以進行 Southbound 通訊。 這些互動會使用不同的通訊協定。 因此，根據網路控制站用來與裝置通訊的裝置和通訊協定類型，驗證、授權和加密會有不同的需求。

下表提供與不同 southbound 裝置進行網路控制站互動的相關資訊。

| Southbound 裝置/服務 | Protocol              | 使用的驗證    |
|---------------------------|-----------------------|------------------------|
| 軟體負載平衡器    | WCF （MUX）、TCP （主機） | 憑證           |
| 防火牆                  | OVSDB                 | 憑證           |
| 閘道                   | WinRM                 | Kerberos，憑證 |
| 虛擬網路        | OVSDB、WCF            | 憑證           |
| 使用者定義的路由      | OVSDB                 | 憑證           |

針對上述每個通訊協定，下一節將說明這兩種通訊機制。

### <a name="authentication"></a>驗證

針對 Southbound 通訊，會使用下列通訊協定和驗證方法。

1. **WCF/TCP/OVSDB**。 針對這些通訊協定，會使用 X509 憑證來執行驗證。 網路控制站和對等軟體負載平衡\( \(SLB\)多工\)器 MUX/host 機器會將其憑證分別提供給彼此進行相互驗證。 每個憑證都必須受到遠端對等的信任。

    針對 southbound authentication，您可以使用設定為加密與 Northbound 用戶端通訊的相同 SSL 憑證。 您也必須在 SLB MUX 和主機裝置上設定憑證。 憑證主體名稱必須與裝置的 DNS 名稱相同。

2. **WinRM**。 針對此通訊協定，會針對加入網域的\(電腦\)使用 Kerberos，並針對未加入\(網域的電腦\)使用憑證來執行驗證。

### <a name="authorization"></a>Authorization

針對 Southbound 通訊，會使用下列通訊協定和授權方法。

1. **WCF/TCP**。 針對這些通訊協定，授權是以對等實體的主體名稱為基礎。 網路控制站會儲存對等裝置 DNS 名稱，並使用它來進行授權。 此 DNS 名稱必須符合憑證中裝置的主體名稱。 同樣地，網路控制卡憑證必須符合儲存在對等裝置上的網路控制站 DNS 名稱。

2. **WinRM**。 如果使用 Kerberos，則 WinRM 用戶端帳戶必須存在於 Active Directory 或伺服器上本機系統管理員群組中的預先定義群組中。 如果使用憑證，用戶端會向伺服器出示憑證，而伺服器是使用主體名稱/簽發者來授權，而伺服器則會使用對應的使用者帳戶來執行驗證。

3.  **OVSDB**。 沒有提供此通訊協定的授權。

### <a name="encryption"></a>加密

針對 Southbound 通訊，會將下列加密方法用於通訊協定。

1. **WCF/TCP/OVSDB**。 針對這些通訊協定，會使用在用戶端或伺服器上註冊的憑證來執行加密。

2. **WinRM**。 WinRM 流量預設會使用 Kerberos 安全性支援提供者\(SSP\)進行加密。 您可以在 WinRM 伺服器上以 SSL 的形式設定其他加密。
