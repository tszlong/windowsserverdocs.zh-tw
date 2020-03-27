---
title: 規劃 Always On VPN 部署
description: 本主題提供在 Windows Server 2016 中部署 Always On VPN 的規劃指示。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: lizross
author: eross-msft
ms.date: 11/05/2018
ms.openlocfilehash: e0e061a38170242a3808fbad0c82a4154bf9c536
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313300"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>步驟 1。 規劃 Always On VPN 部署

>適用于： Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一步：** 瞭解部署 Always On VPN 的工作流程](always-on-vpn-deploy-deployment.md)
- [**下一步：** 步驟2。設定伺服器基礎結構](vpn-deploy-server-infrastructure.md)

在此步驟中，您會開始規劃和準備您的 Always On VPN 部署。 在您打算使用做為 VPN 伺服器的電腦上安裝遠端存取服務器角色之前，請執行下列工作。 適當規劃之後，您可以部署 Always On VPN，並選擇性地使用 Azure AD 設定 VPN 連線的條件式存取。

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]

## <a name="prepare-the-remote-access-server"></a>準備遠端存取服務器

您必須在用來做為 VPN 伺服器的電腦上執行下列動作：

- **請確定 VPN 伺服器的軟體和硬體設定正確**。 在您打算用來做為遠端存取 VPN 伺服器的電腦上安裝 Windows Server 2016。 此伺服器必須安裝兩個實體網路介面卡，一個用於連線至外部周邊網路，另一個用來連線到內部周邊網路。

- **識別連線到網際網路的網路介面卡，以及哪些網路介面卡連接到您的私人網路**。 使用公用 IP 位址來設定面向網際網路的網路介面卡，而面向內部網路的介面卡可以使用區域網路的 IP 位址。

    >[!TIP]
    >如果您不想使用周邊網路上的公用 IP 位址，您可以使用公用 IP 位址來設定邊緣防火牆，然後設定防火牆將 VPN 連線要求轉寄到 VPN 伺服器。

- 將**VPN 伺服器連線到網路**。 在周邊網路上的邊緣防火牆和周邊防火牆之間安裝 VPN 伺服器。

## <a name="plan-authentication-methods"></a>規劃驗證方法

IKEv2 是[網際網路工程任務要求批註 7296](https://datatracker.ietf.org/doc/rfc7296/)中所述的 VPN 通道通訊協定。 IKEv2 的主要優點是它會容許基礎網路連線的中斷。 例如，如果連線暫時中斷，或使用者將用戶端電腦從一個網路移到另一個，則重新建立網路連線 IKEv2 時，會自動還原 VPN 連線，而不需要使用者介入。

>[!TIP]
>您可以設定遠端存取 VPN 伺服器以支援 IKEv2 連線，同時也停用未使用的通訊協定，這樣可減少伺服器的安全性使用量。 

## <a name="plan-ip-addresses-for-remote-clients"></a>規劃遠端用戶端的 IP 位址

您可以設定 VPN 伺服器，從您設定的靜態位址集區或從 DHCP 伺服器的 IP 位址，將位址指派給 VPN 用戶端。 

## <a name="prepare-the-environment"></a>準備環境

- **請確定您具有設定外部防火牆的許可權，而且您有有效的公用 IP 位址**。 開啟防火牆上的埠以支援 IKEv2 VPN 連線。 您也需要一個公用 IP 位址，以接受來自外部用戶端的連線。

- **選擇 VPN 用戶端的靜態 IP 位址範圍**。 判斷您想要支援的同時 VPN 用戶端數目上限。 此外，請在內部周邊網路上規劃一系列靜態 IP 位址，以符合該需求，亦即*靜態位址集區*。 如果您使用 DHCP 來提供內部 DMZ 的 IP 位址，您可能也需要為 DHCP 中的靜態 IP 位址建立排除。

- **請確定您可以編輯您的公用 DNS 區域**。 將 DNS 記錄新增至您的公用 DNS 網域，以支援 VPN 基礎結構。 

- **請確定所有 VPN 使用者都具有 Active Directory 使用者（AD DS）中的使用者帳戶**。 在使用者可以使用 VPN 連線連線到網路之前，他們必須在 AD DS 中具有使用者帳戶。

## <a name="prepare-routing-and-firewall"></a>準備路由和防火牆 

將 VPN 伺服器安裝在周邊網路內，這會將周邊網路分割成內部和外部周邊網路。 根據您的網路環境而定，您可能需要進行數個路由修改。

- **選擇性設定埠轉送。** 您的邊緣防火牆必須開啟與 IKEv2 VPN 相關聯的埠和通訊協定識別碼，並將其轉送至 VPN 伺服器。 在大部分的環境中，這麼做會要求您設定埠轉送。 將通用資料包協定（UDP）埠500和4500重新導向至 VPN 伺服器。

- **設定路由，讓 DNS 伺服器和 VPN 伺服器可以連線到網際網路**。 此部署使用 IKEv2 和網路位址轉譯（NAT）。 請確定 VPN 伺服器可以連線到所有必要的內部網路和網路資源。 從遠端位置連線的 VPN 連線也無法連線到 VPN 伺服器無法連線的任何網路或資源。

在大部分的環境中，若要連接到新的內部周邊網路，請調整邊緣防火牆和 VPN 伺服器上的靜態路由。 不過，在更複雜的環境中，您可能需要將靜態路由新增至內部路由器，或調整 VPN 伺服器的內部防火牆規則，以及與 VPN 用戶端相關聯的 IP 位址區塊。

## <a name="next-steps"></a>後續步驟

[步驟2。設定伺服器基礎結構](vpn-deploy-server-infrastructure.md)：在此步驟中，您會安裝並設定支援 VPN 所需的伺服器端元件。 伺服器端元件包括設定 PKI，以散發使用者、VPN 伺服器和 NPS 伺服器所使用的憑證。