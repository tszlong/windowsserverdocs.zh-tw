---
title: 網路控制器安全性
description: 您可以使用本主題以了解如何設定 Network Controller 和其他軟體與裝置間的所有通訊的安全性。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ab04d3f528beb037988e6390fe85ad9af219266b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-security"></a>網路控制器安全性

您可以使用本主題以了解如何設定 Network Controller 和其他軟體與裝置間的所有通訊的安全性。 

>[!NOTE]
>概觀網路控制器，請查看[Network Controller](../technologies/network-controller/network-controller.md)。

您可以安全通訊路徑包含在管理平面，Network Controller 虛擬機器 \(VMs\) 叢集，之間上的資料平面 Southbound 通訊叢集通訊 Northbound 通訊。

1. **Northbound 通訊**。 管理平面 SDN\ 能力管理軟體，例如 Windows PowerShell 和 System Center 一樣 Manager \(SCVMM\) network Controller 通訊。 這些管理工具提供您定義原則的網路，並建立目標的網路，您可以針對比較實際網路設定為實際設定目標狀態同位到狀態的能力。

2. **網路控制器叢集通訊**。 當您設定為 Network Controller 叢集節點的三個或更多 Vm 時，這些節點彼此。 此通訊，否則可能會相關同步處理和資料複寫節點或特定的通訊 Network Controller 服務之間上。

3.  **Southbound 通訊**。 Network Controller SDN 基礎結構和其他裝置，例如軟體負載平衡器、閘道和主機上的資料平面通訊。 您可以使用 Network Controller 設定及管理這些 southbound 裝置，使其維持目標狀態，您所設定的網路。

## <a name="northbound-communication"></a>Northbound 通訊

Network Controller 支援 Northbound 通訊驗證、授權及加密。 下列章節如何這些安全性設定提供的資訊。

**驗證**

當您設定的網路控制器 Northbound 通訊驗證時，您可以允許 Network Controller 叢集節點與管理用護端驗證身分與進行通訊的裝置。

Network Controller 支援下列三種管理戶端與 Network Controller 節點間驗證的模式。

>[!Note]
>如果您正在僅部署 Network Controller 的 [系統中心一樣管理員] 中，**Kerberos**支援模式。

1. **Kerberos**。 這兩個管理 client，例如電腦的執行 SCVMM，以及所有的網路控制器叢集節點加入網域帳號用於驗證 Active Directory domain 時，您可以使用 F:kerberos 驗證。

2. **X509**。 X509 是 certificate\ 為基礎的驗證。 您可以使用 X509 Active Directory domain 未加入管理戶端時進行驗證。 若要使用 X509，您必須註冊憑證所有網路控制器叢集節點和管理戶端，且所有節點和管理戶端必須都信任彼此的 ' 憑證。

3. **無**。 當您選擇此模式下時，就會執行管理戶端和 Network Controller 之間不驗證。 此模式只適用於測試目的，並建議您不要 production 環境中使用。 

您可以使用 Windows PowerShell 命令設定 Northbound 通訊的驗證模式**安裝-NetworkController**的**ClientAuthentication**的參數。 

如需詳細資訊，下列主題。 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**授權**

當您設定的網路控制器 Northbound 通訊授權時，您可以允許 Network Controller 叢集節點與管理用護端確認的裝置通訊的受信任，且已參與通訊權限。

針對每個支援 Network Controller 的驗證模式，使用下列的授權方法。

1.  **Kerberos**。 當您使用的 Kerberos 驗證方法時，您可以定義的使用者及授權通訊 Network Controller 的 Active Directory 中建立安全性群組，然後將會在授權的使用者與電腦新增到群組的電腦。 您可以設定 Network Controller 的授權使用安全性群組使用**ClientSecurityGroup**的參數**安裝-NetworkController** Windows PowerShell 命令。 Network Controller 安裝之後，您可以變更安全性群組使用[設定為 NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)命令的參數**-ClientSecurityGroup**。 如果您使用 SCVMM，您必須提供參數安全性群組期間部署。

2.  **X509**。 當您使用 X509 驗證方法、Network Controller 只接受從管理已知的憑證憑證碼 Network Controller 的要求。 您可以將這些憑證碼設定使用**ClientCertificateThumbprint**的參數**安裝-NetworkController** Windows PowerShell 命令。 您可以隨時新增其他 client 憑證碼使用**設定為 NetworkController**命令。

3.  **無**。 當您選擇此模式下時，就會未授權管理戶端與 Network Controller 節點間通訊嘗試執行。 此模式只適用於測試目的，並建議您不要 production 環境中使用。 

如需詳細資訊，下列主題。 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**加密**

Northbound 通訊使用安全通訊端層 \(SSL\) 建立加密的通道管理戶端之間 Network Controller 節點。 SSL 加密進行通訊 Northbound 包含下列需求。

- 所有網路控制器節點都必須包含伺服器的驗證，驗證 Client 目的增強金鑰使用方法 \(EKU\) 擴充功能相同憑證。 

- 管理用用於與 Network Controller 的 URI 必須憑證主體名稱。 必須包含憑證主體名稱，完全完整網域名稱 (FQDN) 或控制器其餘端點網路的 IP 位址。

- 如果網路控制器節點位於不同子網路，憑證主體名稱必須是相同的您使用的值為**RestName**中的參數**安裝-NetworkController** Windows PowerShell 命令。 

- 所有的管理用必須信任 SSL 憑證。

### <a name="ssl-certificate-enrollment-and-configuration"></a>SSL 憑證註冊和設定

您必須手動註冊 Network Controller 節點上的 SSL 憑證。

憑證已退出之後，您可以設定網路的控制器使用憑證的**-伺服器的憑證**的參數**安裝-NetworkController** Windows PowerShell 命令。 如果您已經安裝網路控制器，您可以更新隨時設定使用**設定為 NetworkController**命令。

>[!NOTE]
>如果您使用 SCVMM，您必須為資源庫中新增憑證。 如需詳細資訊，請查看[設定中 VMM fabric SDN 網路控制器](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

## <a name="network-controller-cluster-communication"></a>網路控制器叢集通訊

Network Controller 支援 Network Controller 節點間通訊驗證、授權及加密。 通訊是透過[Windows 通訊基本知識](https://msdn.microsoft.com/library/ms731082.aspx)\(WCF\) 與 TCP。

您可以使用此模式來設定**ClusterAuthentication**的參數**安裝-NetworkControllerCluster** Windows PowerShell 命令。 

如需詳細資訊，請查看[安裝-NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster)。

**驗證**

當您設定的網路控制器叢集通訊驗證時，您可以允許 Network Controller 叢集節點，以驗證身分與進行通訊的其他節點。

Network Controller 支援下列網路控制器節點間驗證的三個模式。

>[!NOTE]
>如果您只使用 SCVMM，部署 Network Controller **Kerberos**支援模式。

1. **Kerberos**。 Active Directory 網域中加入所有網路控制器叢集節點網域帳號用於驗證時，您可以使用 F:kerberos 驗證。

2. **X509**。 X509 是 certificate\ 為基礎的驗證。 您可以使用的 X509 Active Directory domain 未加入驗證時 Network Controller 叢集節點。 若要使用 X509，您必須註冊憑證所有網路控制器叢集節點，以和所有節點必須都信任的憑證。 此外，主體名稱每個節點上退出的憑證必須節點的 DNS 名稱相同。

3. **無**。 當您選擇此模式下時，就會執行 Network Controller 節點之間不驗證。 此模式只適用於測試目的，並建議您不要 production 環境中使用。

**授權**

當您設定的網路控制器叢集通訊授權時，您可以允許 Network Controller 叢集節點驗證的節點與進行通訊的受信任，並讓參與通訊權限。

針對每個支援 Network Controller 的驗證模式，使用下列的授權方法。

1. **Kerberos**。 網路控制器節點接受只從其他 Network Controller 電腦帳號通訊要求。 您可以將這些帳號設定當您使用部署 Network Controller**名稱**的參數[新-NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令。

2. **X509**。 網路控制器節點接受只從其他 Network Controller 電腦帳號通訊要求。 您可以將這些帳號設定當您使用部署 Network Controller**名稱**的參數[新-NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) Windows PowerShell 命令。

3. **無**。 當您選擇此模式下時，就會執行 Network Controller 節點之間不授權。 此模式只適用於測試目的，並建議您不要 production 環境中使用。

**加密**

Network Controller 節點間通訊使用 WCF 傳輸層級加密加密。 這種加密驗證和授權方法 Kerberos 或 X509 時使用的憑證。 如需詳細資訊，下列主題。

- [如何：安全的 Windows 認證服務](https://msdn.microsoft.com/library/ms734673.aspx)
- [如何：安全服務 X.509 使用](https://msdn.microsoft.com/library/ms788968.aspx)。

## <a name="southbound-communication"></a>Southbound 通訊

Network Controller 互動不同類型的裝置 Southbound 通訊。 這些互動使用不同的通訊協定。 而有不同的驗證、授權及加密根據裝置類型和通訊協定進行通訊的裝置使用 Network Controller 的需求。

下表會提供有關的不同 southbound 裝置 Network Controller 互動的資訊。

| Southbound 裝置日服務 | 通訊協定              | 使用驗證    |
|---------------------------|-----------------------|------------------------|
| 軟體負載平衡器    | WCF (MUX)，TCP（主機） | 憑證           |
| 防火牆                  | OVSDB                 | 憑證           |
| 閘道                   | WinRM                 | Kerberos 憑證 |
| Virtual 網路        | OVSDB WCF            | 憑證           |
| 使用者定義路由      | OVSDB                 | 憑證           |

針對每個這些通訊協定，通訊機制是下一節中所述。

**驗證**

Southbound 通訊，用下列通訊協定和的驗證方法。

1. **OVSDB 日 TCP WCF 日**。 適用於這些通訊協定，使用 X509 執行驗證憑證。 同時 Network Controller and 等軟體負載平衡 \(SLB\) Multiplexer \ (MUX\) / 主機上存在彼此互加好友的驗證的憑證。 每個憑證必須信任遠端等。

    Southbound 驗證，您可以使用相同的設定 SSL 憑證來加密 Northbound 戶端的通訊。 您還必須設定 SLB MUX 主機裝置上的憑證。 憑證主體名稱必須是裝置的相同的 DNS 名稱。

2. **WinRM**。 這個通訊協定，利用 Kerberos 執行驗證 \（適用於加入網域 machines\) 並使用的憑證 \（適用於非網域結合 machines\)。

**授權**

Southbound 通訊，會使用授權的方法與下列通訊協定。

1. **TCP WCF 日**。 這些通訊協定，授權根據等實體主體名稱。 Network Controller 儲存對等裝置 DNS 名稱，並使用它來進行授權。 這個 DNS 名稱必須符合憑證的裝置的主體名稱。 同樣地，Network Controller 憑證必須符合儲存對等裝置上的網路控制器 DNS 名稱。

2. **WinRM**。 如果您正在使用 Kerberos，WinRM client account 必須要有預先定義的伺服器上的本機系統管理員群組或 Active Directory 中群組中。 如果您正在使用的憑證，client 呈現的伺服器授權使用主體名稱日發行者，並用伺服器對應的帳號執行驗證伺服器的憑證。

3.  **OVSDB**。 還有不提供這個通訊協定的授權。

**加密**

Southbound 通訊，使用下列的加密方法通訊協定。

1. **OVSDB 日 TCP WCF 日**。 這些通訊協定，加密 client 或伺服器使用憑證已退出的執行。

2. **WinRM**。 預設使用 Kerberos 安全性支援提供者加密 WinRM 流量 \(SSP\)。 您可以設定額外的加密，SSL 的形式 WinRM 伺服器上。
