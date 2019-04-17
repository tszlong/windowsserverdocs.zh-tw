---
title: 規劃 Always On VPN 部署
description: 本主題提供規劃部署 Always On VPN Windows Server 2016 中的指示。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: d629f04abda0ce22deb75e487f5b485f50a60a53
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067130"
---
# 步驟 1. 規劃 Always On VPN 部署

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10


& #171; [**前一個：** 了解有關部署 Always On VPN 工作流程](always-on-vpn-deploy-deployment.md)<br>
& #187; [**下一步：** 步驟 2。設定伺服器基礎結構](vpn-deploy-server-infrastructure.md)

在此步驟中，您開始規劃和準備您的 Always On VPN 部署。 您打算使用為 VPN 伺服器的電腦上安裝遠端存取伺服器角色之前，請執行下列工作。 適當計畫之後，您可以部署 Always On VPN，並選擇性設定的 VPN 連線使用 Azure AD 的條件式存取。 

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]


## 準備遠端存取伺服器

您必須執行下列做為 VPN 伺服器的電腦上： 

- **請確定 VPN 伺服器軟體和硬體設定是正確的**。 在您打算使用做為遠端存取 VPN 伺服器的電腦上安裝 Windows Server 2016。 此伺服器必須有兩個安裝的實體網路介面卡，一個用來連線到外部周邊網路，另一個連線到內部周邊網路。

- **找出連線到網際網路並的網路介面卡連接至您的私人網路的網路介面卡**。 設定面對網際網路與公用 IP 位址，雖然面向近端內部網路介面卡可以使用從本機網路的 IP 位址的網路介面卡。

    >[!TIP]
    >如果您不想在您的適用於周邊網路上使用公用 IP 位址，您可以設定 Edge 防火牆與公用 IP 位址，並接著設定防火牆轉送到 VPN 伺服器的 VPN 連線要求。

- **連線到網路的 VPN 伺服器**。 在 edge 防火牆和周邊防火牆間的周邊網路上安裝的 VPN 伺服器。

## 計劃驗證方法

IKEv2 是 VPN 通道通訊協定[網際網路工程任務推動要求適用於 Comments7296](https://datatracker.ietf.org/doc/rfc7296/)中所述。 IKEv2 的主要優點是它可容許基礎的網路連線中斷。 例如，如果連線中暫時遺失，或在使用者移動的用戶端電腦網路另一個時重新建立網路連線 IKEv2, VPN 連線自動還原 — 而不需要使用者介入。

>[!TIP]
>您可以設定遠端存取 VPN 伺服器支援 IKEv2 連線，同時也停用未使用的通訊協定，這也可以減少伺服器的安全性使用量。 

## 規劃遠端用戶端 IP 位址

您可以設定 VPN 伺服器來指派給 VPN 用戶端從您設定靜態位址集區的地址或從 DHCP 伺服器的 IP 位址。 

## 準備環境

- **請確定您有權限設定外部防火牆，而且您有一個有效的公用 IP 位址**。 開啟防火牆支援 IKEv2 VPN 連線的連接埠。 您也需要接受來自外部用戶端連線的公用 IP 位址。

- **選擇的 VPN 用戶端的靜態 IP 位址範圍**。 決定您想要支援同時 VPN 用戶端的最大數目。 此外，以滿足該需求，也就是*靜態位址集區*內部周邊網路上規劃靜態 IP 位址的範圍。 如果您使用 DHCP 來提供內部 DMZ 上的 IP 位址，您可能也需要在 DHCP 中建立這些靜態 IP 位址的排除範圍。

- **請確定您可以編輯您的公用 DNS 區域**。 新增 DNS 記錄您公用 DNS 網域，以支援 VPN 基礎結構。 

- **請確定所有 VPN 使用者都有 Active Directory 使用者 \(AD DS\) 中的使用者帳戶**。 使用者可以連線到 VPN 連線的網路之前，他們必須 ADDS 中有使用者帳戶。

## 準備路由和防火牆 

安裝適用於周邊網路，這適用於周邊網路分割成內部和外部周邊網路內的 VPN 伺服器。 根據您的網路環境，您可能需要做的幾個路由修改。

- **\(Optional\) 設定連接埠轉送。** Edge 防火牆必須開啟連接埠與通訊協定 IKEv2 VPN 相關聯的識別碼，並將它們轉送至 VPN 伺服器。 在大部分的環境中，這樣做需要您設定連接埠轉送。 將通用資料包通訊協定 (UDP) 連接埠 500 和 4500 重新導向至 VPN 伺服器。

- **設定路由，以便 DNS 伺服器和 VPN 伺服器可以觸達網際網路**。 此部署會使用 IKEv2 和網路位址轉譯 \(NAT\)。 請確定 VPN 伺服器可以觸達所有必要的內部網路和網路資源。 任何網路或無法連線到 VPN 伺服器的資源也是無法連線到 VPN 連線，從遠端位置。

在大部分的環境中，將適用範圍擴及新的內部周邊網路，調整 edge 防火牆和 VPN 伺服器上的靜態路由。 在更複雜的環境中，不過，您可能需要將靜態路由新增到內部路由器或調整的 VPN 伺服器及 VPN 用戶端相關聯的 IP 位址的區塊內部的防火牆規則。

## 後續步驟
[步驟 2。設定伺服器基礎結構](vpn-deploy-server-infrastructure.md)： 在此步驟中，您安裝並設定支援 VPN 所需的伺服器端元件。 伺服器端元件，包括設定 PKI 散發的使用者、 VPN 伺服器和 NPS 伺服器所使用的憑證。 

---