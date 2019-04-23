---
title: 規劃 Always On VPN 部署
description: 本主題提供計劃部署一律開啟 」 VPN Windows Server 2016 中的指示。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: d629f04abda0ce22deb75e487f5b485f50a60a53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881909"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>步驟 1. 計劃一律開啟 」 VPN 部署

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10


&#171;  [**前一個：** 深入了解部署一律開啟 」 VPN 的工作流程](always-on-vpn-deploy-deployment.md)<br>
&#187;  [**下一步:** 步驟 2.設定伺服器基礎結構](vpn-deploy-server-infrastructure.md)

在此步驟中，您開始規劃及準備您的一律開啟 」 VPN 部署。 在您打算使用做為 VPN 伺服器的電腦上安裝遠端存取伺服器角色之前，請執行下列工作。 之後適當的規劃中，您可以部署一律開啟 」 VPN，並選擇性地設定 VPN 連線使用 Azure AD 條件式存取。 

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="prepare-the-remote-access-server"></a>準備遠端存取伺服器

您必須執行下列做為 VPN 伺服器的電腦上： 

- **請確認 VPN 伺服器軟體和硬體組態是否正確**。 在您打算使用做為遠端存取 VPN 伺服器的電腦上安裝 Windows Server 2016。 此伺服器必須安裝兩張實體網路介面卡、 一個用來連接到外部的周邊網路中，而另一個以連線到內部的周邊網路。

- **識別哪個網路介面卡連線到網際網路，以及哪個網路介面卡連接到您的私人網路**。 設定網路介面卡的面向網際網路的公用 IP 位址，雖然面向的內部網路介面卡可以使用區域網路的 IP 位址。

    >[!TIP]
    >如果您不想在周邊網路上使用的公用 IP 位址，您可以使用公用 IP 位址，設定邊緣防火牆，然後設定 VPN 連線要求轉送到 VPN 伺服器在防火牆。

- **連線到網路的 VPN 伺服器**。 安裝在周邊網路之間在邊緣防火牆與周邊防火牆上的 VPN 伺服器。

## <a name="plan-authentication-methods"></a>規劃驗證方法

IKEv2 是 VPN 通道通訊協定中所述[網際網路工程任務推動小組要求註解 7296](https://datatracker.ietf.org/doc/rfc7296/)。 IKEv2 的主要優點是它可容許在基礎的網路連線中斷。 例如，如果暫時無法使用連接中，或使用者將移至用戶端電腦，從一個網路之間時重新建立網路連線 IKEv2 VPN 連線自動還原 — 不需要使用者介入。

>[!TIP]
>您可以設定遠端存取 VPN 伺服器支援 IKEv2 連線，同時也停用未使用的通訊協定，可減少伺服器的安全性使用量。 

## <a name="plan-ip-addresses-for-remote-clients"></a>規劃遠端用戶端的 IP 位址

您可以設定 VPN 伺服器指派給 VPN 用戶端從您設定靜態位址集區的位址或 DHCP 伺服器的 IP 位址。 

## <a name="prepare-the-environment"></a>準備環境

- **請確定您有權限可設定您的外部防火牆，而且您有有效的公用 IP 位址**。 開啟防火牆，以支援 IKEv2 VPN 連線的連接埠。 您也需要以接受來自外部用戶端連線的公用 IP 位址。

- **VPN 用戶端選擇的靜態 IP 位址範圍**。 決定您想要支援的同時 VPN 用戶端的數目上限。 此外，規劃符合這項需求，也就是內部的周邊網路上的 靜態 IP 位址範圍*靜態位址集區*。 如果您使用 DHCP 提供內部周邊網路上的 IP 位址時，您可能也需要在 DHCP 中建立這些靜態 IP 位址的排除範圍。

- **請確定您可以編輯您的公用 DNS 區域**。 加入以支援的 VPN 基礎結構在公用 DNS 網域的 DNS 記錄。 

- **請確定所有的 VPN 使用者都擁有使用者帳戶在 Active Directory 使用者\(AD DS\)**。 使用者可以連線到 VPN 連線的網路之前，它們必須具有 AD DS 中的使用者帳戶。

## <a name="prepare-routing-and-firewall"></a>準備路由和防火牆 

安裝在資料分割到內部和外部的周邊網路的周邊網路的周邊網路內 VPN 伺服器。 根據您的網路環境，您可能需要進行數個路由的修改。

- **\(選擇性\)設定連接埠轉送。** 邊緣防火牆必須開啟連接埠和通訊協定與 IKEv2 VPN 相關聯的識別碼，並將其轉送至 VPN 伺服器。 在大部分的環境中，這麼做還需要您設定連接埠轉送。 將通用資料包通訊協定 (UDP) 連接埠 500 和 4500 重新導向到 VPN 伺服器。

- **設定路由，讓 DNS 伺服器和 VPN 伺服器可以連線到網際網路**。 此部署會使用 IKEv2 和網路位址轉譯\(NAT\)。 請確認 VPN 伺服器可以連線到所有必要的內部網路和網路資源。 任何網路或無法連線到 VPN 伺服器的資源也是無法連上透過 VPN 連線，從遠端位置。

在大部分的環境中，若要連線到新的內部周邊網路，調整在邊緣防火牆與 VPN 伺服器上的靜態路由。 在更複雜的環境中，不過，您可能需要將靜態路由新增至內部路由器或調整 VPN 伺服器和 VPN 用戶端相關聯的 IP 位址區塊的內部防火牆規則。

## <a name="next-step"></a>後續步驟
[步驟 2。設定伺服器基礎結構](vpn-deploy-server-infrastructure.md):在此步驟中，您可以安裝及設定支援的 VPN 所需的伺服器端元件。 伺服器端元件包括設定將使用者、 VPN 伺服器和 NPS 伺服器所使用的憑證的 PKI。 

---