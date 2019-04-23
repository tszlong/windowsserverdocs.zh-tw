---
title: 網路控制卡安全性
description: 您可以使用本主題以了解如何設定網路控制站和其他軟體和裝置之間的所有通訊的安全性。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: fccd6ee1ba734d72629264bd5b3bb7d93ddcdb70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884759"
---
# <a name="secure-the-network-controller"></a>保護網路控制卡

本主題中，了解如何設定安全性之間的所有通訊[網路控制卡](../technologies/network-controller/network-controller.md)和其他軟體和裝置。 

您可以保護的通訊路徑包含 Northbound 管理平面，網路控制站虛擬機器之間的叢集通訊的通訊\(Vm\)叢集中，Southbound 通訊的資料平面。

1. **Northbound 通訊**。 管理平面使用 SDN 網路控制站進行通訊\-功能的管理軟體，例如 Windows PowerShell 和 System Center Virtual Machine Manager \(SCVMM\)。 這些管理工具為您提供定義網路原則，並建立的網路時，針對中，您可以比較實際的網路設定，以實際的組態設定成同位檢查的目標狀態的目標狀態的能力。

2. **網路控制站叢集通訊**。 當您為網路控制卡的叢集節點設定三個或多個 Vm 時，這些節點會與彼此通訊。 此通訊可能會與同步處理和複寫的資料跨節點或特定網路控制卡服務之間的通訊。

3.  **Southbound 通訊**。 網路控制站會在 SDN 基礎結構與其他裝置，例如軟體負載平衡器、 閘道和主機電腦的資料平面上進行通訊。 若要設定及管理這些 southbound 的裝置，讓它們維護您的網路設定的目標狀態，您可以使用網路控制站。


## <a name="northbound-communication"></a>Northbound 通訊

網路控制器 Northbound 通訊支援驗證、 授權和加密。 下列各節提供有關如何設定這些安全性設定的資訊。

### <a name="authentication"></a>驗證

當您設定網路控制器 Northbound 通訊的驗證時，您必須允許網路控制卡的叢集節點和管理用戶端，以確認正在與其通訊的裝置身分識別。

網路控制站支援下列三種管理用戶端與網路控制卡節點之間的驗證模式。

>[!Note]
>如果您將部署網路控制站與 「 System Center Virtual Machine Manager 」 中，只有**Kerberos**支援模式。

1. **Kerberos**。 加入 Active Directory 網域中的管理用戶端和網路控制站的所有叢集節點時，請使用 Kerberos 驗證。 Active Directory 網域必須擁有用於驗證的網域帳戶。

2. **X509**。 使用 X509 憑證\-為基礎的管理用戶端未加入 Active Directory 網域的驗證。 您必須註冊到所有網路控制卡的叢集節點和管理用戶端的憑證。 此外，所有節點和管理用戶端必須都信任其他人的憑證。

3. **None**： 用於無測試用途在測試環境中，因此，不建議在生產環境中使用。 當您選擇這個模式時，沒有任何節點管理用戶端之間執行的驗證。

您也可以使用 Windows PowerShell 命令來設定 「 Northbound 通訊的驗證模式**[安裝 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** 使用_ClientAuthentication_參數。 


### <a name="authorization"></a>Authorization

當您設定網路控制器 Northbound 通訊的授權時，允許網路控制卡的叢集節點和管理用戶端，以確認正在與其通訊的裝置是否受信任，且具有參與權限通訊。

針對每個網路控制卡所支援的驗證模式中使用下列授權方法。

1.  **Kerberos**。 當您使用 Kerberos 驗證方法時，您可以定義的使用者和電腦授權 Active Directory 中建立安全性群組，然後再新增至群組的 授權的使用者和電腦與網路控制站通訊。 您可以設定要用於授權中的安全性群組，使用網路控制卡_ClientSecurityGroup_的參數**[安裝 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** Windows PowerShell 命令。 安裝網路控制站之後, 您可以變更的安全性群組，使用**[組 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** 命令與參數 _-ClientSecurityGroup_. 如果使用 SCVMM，您必須在部署期間做為參數提供的安全性群組。

2.  **X509**。 當您使用 X509 驗證方法，網路控制站只接受來自網路控制卡已知其憑證指紋的管理用戶端要求。 您可以使用來設定這些憑證指紋_ClientCertificateThumbprint_的參數**[安裝 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)**  Windows PowerShell 命令。 您也可以使用在任何時候新增其他用戶端憑證指紋**[組 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** 命令。

3.  **None**： 當您選擇這個模式時，沒有任何節點管理用戶端之間執行的驗證。 用於無測試用途在測試環境中，因此，不建議在生產環境中使用。 


### <a name="encryption"></a>加密

Northbound 通訊使用安全通訊端層\(SSL\)來建立管理用戶端與網路控制卡節點之間的加密的通道。 Northbound 通訊的 SSL 加密包括下列需求：

- 網路控制站的所有節點必須要都有相同的憑證在 增強金鑰使用方法包含伺服器驗證和用戶端驗證用途\(EKU\)延伸模組。 

- 管理用戶端與網路控制站進行通訊所用的 URI 必須是憑證主體名稱。 憑證主體名稱必須包含完整格式網域名稱 (FQDN) 或網路控制站的 REST 端點的 IP 位址。

- 如果網路控制卡節點位於不同子網路，其憑證的主體名稱必須是所使用的值相同_RestName_中的參數**安裝 NetworkController** WindowsPowerShell 命令。 

- 所有的管理用戶端必須信任的 SSL 憑證。


### <a name="ssl-certificate-enrollment-and-configuration"></a>SSL 憑證註冊和組態

您必須手動註冊節點網路控制站上的 SSL 憑證。

註冊憑證之後，您可以設定要使用的憑證與網路控制卡 **-ServerCertificate**的參數**安裝 NetworkController** Windows PowerShell 命令. 如果您已安裝網路控制站，您可以使用更新的設定，隨時**組 NetworkController**命令。

>[!NOTE]
>如果您使用 SCVMM，您必須新增為程式庫資源的憑證。 如需詳細資訊，請參閱 <<c0> [ 設定 VMM 光纖中的 SDN 網路控制站](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

## <a name="network-controller-cluster-communication"></a>網路控制站的叢集通訊

網路控制卡支援網路控制卡節點之間通訊的驗證、 授權和加密。 通訊是透過[Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\)和 TCP。

您可以設定此模式搭配**ClusterAuthentication**的參數**安裝 NetworkControllerCluster** Windows PowerShell 命令。 

如需詳細資訊，請參閱 <<c0> [ 安裝 NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster)。

### <a name="authentication"></a>驗證

當您設定網路控制站叢集通訊的驗證時，您會讓網路控制站驗證的身分識別與其通訊的其他節點的叢集節點。

網路控制站支援下列三種網路控制卡節點之間的驗證模式。

>[!NOTE]
>如果您部署網路控制站只會使用 SCVMM **Kerberos**支援模式。

1. **Kerberos**。 網路控制站的所有叢集節點都加入 Active Directory 網域，用來進行驗證的網域帳戶時，您可以使用 Kerberos 驗證。

2. **X509**。 X509 是憑證\-型驗證。 您可以使用的 X509 驗證網路控制卡的叢集節點時未加入 Active Directory 網域。 若要使用 X509，您必須註冊憑證至所有叢集節點網路控制站，以及所有節點都必須都信任憑證。 此外，每個節點註冊憑證的主體名稱必須是節點的 DNS 名稱相同。

3. **None**： 當您選擇這個模式時，沒有任何網路控制卡節點之間執行的驗證。 這種模式僅供測試之用，並不建議在生產環境中使用。

### <a name="authorization"></a>Authorization

當您設定網路控制站叢集通訊的授權時，您就會允許將叢集節點網路控制站來確認正在與其通訊的節點是受信任，並有參與通訊的權限。

針對每個網路控制卡所支援的驗證模式，請使用下列的授權方法。

1. **Kerberos**。 網路控制站節點通訊只接受來自要求其他網路控制站電腦帳戶。 您可以設定這些帳戶，當您使用部署網路控制卡**名稱**的參數[新增 NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令。

2. **X509**。 網路控制站節點通訊只接受來自要求其他網路控制站電腦帳戶。 您可以設定這些帳戶，當您使用部署網路控制卡**名稱**的參數[新增 NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令。

3. **None**： 當您選擇這個模式時，會無法執行網路控制卡節點之間的授權。 這種模式僅供測試之用，並不建議在生產環境中使用。

### <a name="encryption"></a>加密

使用 WCF 傳輸層級加密來加密網路控制卡節點之間的通訊。 這種形式的加密使用驗證和授權方法 Kerberos 或 X509 憑證。 如需詳細資訊，請參閱下列主題。

- [如何：使用 Windows 認證的服務安全](https://msdn.microsoft.com/library/ms734673.aspx)
- [如何：保護使用 X.509 憑證的服務安全](https://msdn.microsoft.com/library/ms788968.aspx)。

## <a name="southbound-communication"></a>Southbound 通訊

網路控制站會與不同類型的 Southbound 通訊的裝置互動。 這些互動使用不同的通訊協定。 基於這個原因，有不同的需求，進行驗證、 授權和加密，視裝置與網路控制站用來與裝置通訊的通訊協定的類型而定。

下表提供與不同 southbound 裝置的網路控制卡互動的相關資訊。

| Southbound 裝置/服務 | 通訊協定              | 使用驗證    |
|---------------------------|-----------------------|------------------------|
| 軟體負載平衡器    | WCF (MUX)、 TCP （主機） | 憑證           |
| 防火牆                  | OVSDB                 | 憑證           |
| 閘道                   | WinRM                 | Kerberos、 憑證 |
| 虛擬網路        | OVSDB WCF            | 憑證           |
| 使用者定義路由      | OVSDB                 | 憑證           |

針對每個這些通訊協定，通訊機制是下一節中所述。

### <a name="authentication"></a>驗證

Southbound 的通訊，會使用下列通訊協定和驗證方法。

1. **WCF/TCP/OVSDB**。 這些通訊協定，驗證會使用 X509 憑證。 網路控制站和對等軟體負載平衡\(SLB\)解多工器\(MUX \) /主機機器呈現其彼此進行相互驗證的憑證。 每個憑證必須由遠端對等電腦的信任。

    Southbound 驗證，您可以使用相同設定的 SSL 憑證來加密與 Northbound 的用戶端通訊。 您也必須在 SLB MUX 和主機裝置上設定憑證。 憑證主體名稱必須與裝置的 DNS 名稱相同。

2. **WinRM**。 這項通訊協定，使用 Kerberos 來執行驗證\(針對已加入網域的機器\)以及使用憑證\(非網域聯結機器\)。

### <a name="authorization"></a>Authorization

Southbound 的通訊，會使用下列通訊協定和授權方法。

1. **WCF/TCP**。 這些通訊協定，授權根據對等實體的主體名稱。 網路控制站會儲存對等裝置 DNS 名稱，並使用它來進行授權。 這個 DNS 名稱必須符合憑證中裝置的主體名稱。 同樣地，網路控制站憑證必須符合儲存在對等裝置上的網路控制站 DNS 名稱。

2. **WinRM**。 如果使用 Kerberos，WinRM 用戶端帳戶必須位於 Active Directory 中或在伺服器上本機 Administrators 群組中預先定義的群組。 如果正在使用的憑證，用戶端會出示的憑證伺服器，伺服器可讓您授權使用主體名稱/簽發者，以及伺服器是使用對應的使用者帳戶來執行驗證。

3.  **OVSDB**。 沒有任何授權提供給此通訊協定。

### <a name="encryption"></a>加密

Southbound 通訊，如下列的加密方法會用於通訊協定。

1. **WCF/TCP/OVSDB**。 這些通訊協定，加密會使用用戶端或伺服器上的已註冊的憑證。

2. **WinRM**。 使用 Kerberos 安全性支援提供者的預設 WinRM 流量會加密\(SSP\)。 您可以設定額外的加密，WinRM 伺服器上的 SSL，表單中。
