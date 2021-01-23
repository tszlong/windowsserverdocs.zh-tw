---
title: 網路控制卡安全性
description: 您可以使用本主題來瞭解如何設定網路控制站與其他軟體和裝置之間的所有通訊安全性。
manager: grcusanz
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/30/2018
ms.openlocfilehash: de9f178d6f70478de162db9c063d414b021c9d1e
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716474"
---
# <a name="secure-the-network-controller"></a>保護網路控制卡

>適用於：Windows Server 2019、Windows Server 2016

在本主題中，您將瞭解如何設定 [網路控制](../technologies/network-controller/network-controller.md) 站與其他軟體和裝置之間的所有通訊安全性。

您可以保護的通訊路徑包括管理平面上的 Northbound 通訊、叢集中的網路控制站虛擬機器 vm 之間的叢集通訊 \( \) ，以及資料平面上的 Southbound 通訊。

1. **Northbound 通訊**。 網路控制站會在管理平面上進行通訊，以及支援 SDN 的 \- 管理軟體，例如 Windows PowerShell 和 System Center Virtual Machine Manager \( SCVMM \) 。 這些管理工具可讓您定義網路原則，並建立網路的目標狀態，讓您可以比較實際的網路設定，讓實際的設定與目標狀態保持同位。

2. **網路控制** 站叢集通訊。 當您將三部或多部 Vm 設定為網路控制卡叢集節點時，這些節點會彼此通訊。 這項通訊可能與跨節點的資料進行同步處理和複寫，或是網路控制站服務之間的特定通訊有關。

3.  **Southbound 通訊**。 網路控制站會以 SDN 基礎結構和其他裝置（如軟體負載平衡器、閘道和主機電腦）在資料平面上進行通訊。 您可以使用網路控制站來設定及管理這些 southbound 裝置，讓它們維持您針對網路所設定的目標狀態。


## <a name="northbound-communication"></a>Northbound 通訊

網路控制站支援驗證、授權和加密以進行 Northbound 通訊。 下列各節提供有關如何設定這些安全性設定的資訊。

### <a name="authentication"></a>驗證

當您設定網路控制站 Northbound 通訊的驗證時，您可以允許網路控制站叢集節點和管理用戶端確認與其通訊之裝置的身分識別。

網路控制站支援管理用戶端和網路控制器節點之間的下列三種驗證模式。

>[!Note]
>如果您要使用 System Center Virtual Machine Manager 部署網路控制站，則僅支援 **Kerberos** 模式。

1. **Kerberos**。 將管理用戶端和所有網路控制站叢集節點聯結至 Active Directory 網域時，請使用 Kerberos 驗證。 Active Directory 網域必須有用於驗證的網域帳戶。

2. **X509**。 \-針對未加入 Active Directory 網域的管理用戶端，使用 X509 進行以憑證為基礎的驗證。 您必須將憑證註冊到所有網路控制站叢集節點和管理用戶端。 此外，所有節點和管理用戶端都必須信任彼此的憑證。

3. **None**： 在測試環境中使用 None 作為測試用途，因此不建議在生產環境中使用。 當您選擇此模式時，不會在節點與管理用戶端之間執行驗證。

您可以使用 Windows PowerShell 命令 **[安裝-NetworkController](/powershell/module/networkcontroller/install-networkcontroller)** 和 _ClientAuthentication_ 參數，設定 Northbound 通訊的驗證模式。


### <a name="authorization"></a>授權

當您設定網路控制站 Northbound 通訊的授權時，您可以允許網路控制站叢集節點和管理用戶端確認與其通訊的裝置是否受信任，且具有參與通訊的許可權。

針對網路控制站所支援的每個驗證模式，使用下列授權方法。

1.  **Kerberos**。 當您使用 Kerberos 驗證方法時，您可以在 Active Directory 中建立安全性群組，然後將授權的使用者和電腦新增至群組，以定義授權與網路控制站通訊的使用者和電腦。 您可以使用 **[安裝 NetworkController](/powershell/module/networkcontroller/install-networkcontroller)** Windows PowerShell 命令的 _ClientSecurityGroup_ 參數，將網路控制站設定為使用安全性群組進行授權。 在安裝網路控制站之後，您可以使用 **[NetworkController](/powershell/module/networkcontroller/Set-NetworkController)** 命令搭配 _ClientSecurityGroup_ 參數來變更安全性群組。 如果使用 SCVMM，您必須在部署期間提供安全性群組做為參數。

2.  **X509**。 當您使用 X509 驗證方法時，網路控制站只接受來自管理用戶端的要求，而這些用戶端的憑證指紋是網路控制站已知的。 您可以使用 **[安裝 NetworkController](/powershell/module/networkcontroller/install-networkcontroller)** Windows PowerShell 命令的 _ClientCertificateThumbprint_ 參數來設定這些指紋。 您隨時可以使用 **[NetworkController](/powershell/module/networkcontroller/Set-NetworkController)** 命令來新增其他用戶端指紋。

3.  **None**： 當您選擇此模式時，不會在節點與管理用戶端之間執行驗證。 在測試環境中使用 None 作為測試用途，因此不建議在生產環境中使用。


### <a name="encryption"></a>加密

Northbound 通訊使用安全通訊端層 \( SSL \) 來建立管理用戶端和網路控制站節點之間的加密通道。 Northbound 通訊的 SSL 加密包含下列需求：

- 所有網路控制卡節點都必須具有相同的憑證，其中包含伺服器驗證和用戶端驗證目的（在增強金鑰使用方式 \( EKU 延伸中） \) 。

- 管理用戶端用來與網路控制站通訊的 URI 必須是憑證主體名稱。 憑證主體名稱必須包含 (FQDN) 的完整功能變數名稱，或網路控制站 REST 端點的 IP 位址。

- 如果網路控制站節點位於不同的子網，其憑證的主體名稱必須與 **NetworkController** Windows PowerShell 命令中用於 _RestName_ 參數的值相同。

- 所有管理用戶端都必須信任 SSL 憑證。


### <a name="ssl-certificate-enrollment-and-configuration"></a>SSL 憑證註冊和設定

您必須在網路控制站節點上手動註冊 SSL 憑證。

註冊憑證之後，您可以設定網路控制站，以使用憑證搭配 **NetworkController** Windows PowerShell 命令的 **-ServerCertificate** 參數。 如果您已安裝網路控制卡，您可以在任何時間使用 NetworkController 命令來更新 **設定** 。

>[!NOTE]
>如果您使用的是 SCVMM，則必須將憑證新增為程式庫資源。 如需詳細資訊，請參閱 [在 VMM 網狀架構中設定 SDN 網路控制](/system-center/vmm/sdn-controller)站。

## <a name="network-controller-cluster-communication"></a>網路控制站叢集通訊

網路控制站支援在網路控制站節點之間進行通訊的驗證、授權和加密。 通訊是透過 [Windows Communication Foundation](/dotnet/framework/wcf/whats-wcf) \( WCF \) 和 TCP 來進行。

您可以使用 **安裝 NetworkControllerCluster** Windows PowerShell 命令的 **ClusterAuthentication** 參數來設定此模式。

如需詳細資訊，請參閱 [安裝-NetworkControllerCluster](/powershell/module/networkcontroller/install-networkcontrollercluster)。

### <a name="authentication"></a>驗證

當您設定網路控制站叢集通訊的驗證時，您可以允許網路控制站叢集節點確認與其通訊之其他節點的身分識別。

網路控制站支援在網路控制站節點之間進行下列三種驗證模式。

>[!NOTE]
>如果您使用 SCVMM 部署網路控制站，則僅支援 **Kerberos** 模式。

1. **Kerberos**。 當所有網路控制卡叢集節點都聯結至 Active Directory 網域，且使用網域帳戶進行驗證時，您可以使用 Kerberos 驗證。

2. **X509**。 X509 是以憑證為 \- 基礎的驗證。 當網路控制器叢集節點未加入 Active Directory 網域時，您可以使用 X509 驗證。 若要使用 X509，您必須將憑證註冊到所有的網路控制站叢集節點，而且所有節點都必須信任該憑證。 此外，在每個節點上註冊之憑證的主體名稱必須與節點的 DNS 名稱相同。

3. **None**： 當您選擇此模式時，網路控制站節點之間不會執行任何驗證。 此模式僅供測試用途，不建議用於生產環境。

### <a name="authorization"></a>授權

當您設定網路控制站叢集通訊的授權時，您可以允許網路控制站叢集節點確認與其通訊的節點受信任，且具有參與通訊的許可權。

針對網路控制站所支援的每個驗證模式，會使用下列授權方法。

1. **Kerberos**。 網路控制站節點只接受來自其他網路控制站電腦帳戶的通訊要求。 當您使用 [NetworkControllerNodeObject](/powershell/module/networkcontroller/new-networkcontrollernodeobject) Windows PowerShell 命令的 **Name** 參數部署網路控制站時，可以設定這些帳戶。

2. **X509**。 網路控制站節點只接受來自其他網路控制站電腦帳戶的通訊要求。 當您使用 [NetworkControllerNodeObject](/powershell/module/networkcontroller/new-networkcontrollernodeobject) Windows PowerShell 命令的 **Name** 參數部署網路控制站時，可以設定這些帳戶。

3. **None**： 當您選擇此模式時，網路控制站節點之間不會執行任何授權。 此模式僅供測試用途，不建議用於生產環境。

### <a name="encryption"></a>加密

網路控制站節點之間的通訊會使用 WCF 傳輸層級加密來加密。 當驗證和授權方法是 Kerberos 或 X509 憑證時，就會使用這種形式的加密。 如需詳細資訊，請參閱下列主題。

- [作法：使用 Windows 認證來確保服務安全](/dotnet/framework/wcf/how-to-secure-a-service-with-windows-credentials)
- [如何：使用 X.509 憑證保護服務的安全](/dotnet/framework/wcf/feature-details/how-to-secure-a-service-with-an-x-509-certificate)。

## <a name="southbound-communication"></a>Southbound 通訊

網路控制站會與不同類型的裝置互動以進行 Southbound 通訊。 這些互動使用不同的通訊協定。 因此，根據網路控制站用來與裝置通訊的裝置和通訊協定類型，驗證、授權和加密會有不同的需求。

下表提供與不同 southbound 裝置進行網路控制站互動的相關資訊。

| Southbound 裝置/服務 | 通訊協定              | 使用的驗證    |
|---------------------------|-----------------------|------------------------|
| 軟體負載平衡器    | WCF (MUX) 、TCP (主機)  | 憑證           |
| 防火牆                  | OVSDB                 | 憑證           |
| 閘道                   | WinRM                 | Kerberos，憑證 |
| 虛擬網路        | OVSDB、WCF            | 憑證           |
| 使用者定義路由      | OVSDB                 | 憑證           |

針對這些通訊協定，下一節將說明通訊機制。

### <a name="authentication"></a>驗證

針對 Southbound 通訊，會使用下列通訊協定和驗證方法。

1. **WCF/TCP/OVSDB**。 針對這些通訊協定，則會使用 X509 憑證來執行驗證。 網路控制站和對等軟體負載平衡 SLB 多工器 \( \) \( MUX \) /host 機器會將其憑證呈現給彼此以進行相互驗證。 每個憑證都必須受到遠端對等的信任。

    針對 southbound 驗證，您可以使用設定用來加密與 Northbound 用戶端通訊的相同 SSL 憑證。 您也必須設定 SLB MUX 和主機裝置上的憑證。 憑證的主體名稱必須與裝置的 DNS 名稱相同。

2. **WinRM**。 針對此通訊協定，會針對已 \( 加入網域的電腦使用 Kerberos \) ，以及 \( 針對未加入網域的電腦使用憑證來執行驗證 \) 。

### <a name="authorization"></a>授權

針對 Southbound 通訊，會使用下列通訊協定和授權方法。

1. **WCF/TCP**。 針對這些通訊協定，授權是以對等實體的主體名稱為基礎。 網路控制站會儲存對等裝置 DNS 名稱，並使用它來進行授權。 此 DNS 名稱必須符合憑證中裝置的主體名稱。 同樣地，網路控制卡憑證必須符合儲存在對等裝置上的網路控制站 DNS 名稱。

2. **WinRM**。 如果使用的是 Kerberos，則 WinRM 用戶端帳戶必須存在於 Active Directory 的預先定義群組或伺服器上的本機 Administrators 群組中。 如果使用憑證，用戶端會向伺服器出示憑證，而伺服器會使用主體名稱/簽發者來授權，而伺服器則會使用對應的使用者帳戶來執行驗證。

3.  **OVSDB**。 未針對此通訊協定提供授權。

### <a name="encryption"></a>加密

針對 Southbound 通訊，會使用下列加密方法來進行通訊協定。

1. **WCF/TCP/OVSDB**。 針對這些通訊協定，會使用在用戶端或伺服器上註冊的憑證來執行加密。

2. **WinRM**。 WinRM 流量預設會使用 Kerberos 安全性支援提供者 SSP 進行加密 \( \) 。 您可以在 WinRM 伺服器上以 SSL 的形式設定額外的加密。