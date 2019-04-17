---
title: 設定 RADIUS 戶端
description: 本主題提供適用於 Windows Server 2016 中的網路原則伺服器設定 RADIUS 戶端的相關資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9af3189199c8282394ca34f181a90b4290c55044
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-radius-clients"></a>設定 RADIUS 戶端

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以將網路存取伺服器設定為在 NPS RADIUS 戶端使用本主題。

當您新增新的網路存取伺服器 \（VPN 伺服器，wireless 存取點，驗證切換或撥號 server\）到您的網路，您必須為中 NPS RADIUS client 新增伺服器，然後設定 RADIUS client 具有 NPS 伺服器通訊。

>[!IMPORTANT]
>Client 電腦和裝置，例如膝上型電腦、平板電腦、手機與其他執行 client 作業系統的電腦不是 RADIUS 戶端。 RADIUS 戶端的網路存取伺服器-wireless 存取點，例如 802.1 X 能力切換、virtual 私人網路 (VPN) 伺服器、和撥號伺服器-因為它們可以使用 RADIUS 通訊協定進行通訊 RADIUS 伺服器，例如網路原則伺服器 \(NPS\) 伺服器。

這個步驟也是必要時 NPS 伺服器 NPS proxy 設定遠端 RADIUS 伺服器群組成員。 您必須在這個情況，除了執行這項工作 NPS proxy 中的步驟執行下列動作：

- NPS proxy，設定，其中包含伺服器 NPS RADIUS 遠端伺服器群組。
- NPS 遠端伺服器，將設定為 RADIUS client 的 NPS proxy。

若要執行此主題中的程序，您必須至少網路存取伺服器 \（VPN 伺服器，wireless 存取點，驗證切換或撥號 server\）或 NPS proxy 實際安裝在您的網路。

## <a name="configure-the-network-access-server"></a>設定網路的存取伺服器

若要使用的網路存取伺服器設定具有 NPS 使用此程序。 當部署 RADIUS 戶端為網路存取伺服器 (Nas) 時，您必須設定戶端與 Nas 位置設定為戶端 NPS 伺服器通訊。

此程序提供相關的設定，您應該使用它們來設定您 Nas; 一般指導方針適用於特定如何設定您的部署您網路的裝置上的指示，請查看您 NAS product 文件。

### <a name="to-configure-the-network-access-server"></a>若要設定的網路存取伺服器

1. 在 NAS，在**RADIUS 設定**，請選取**RADIUS 驗證**使用者資料流通訊協定 (UDP) 連接埠**1812 年**和**RADIUS 計量**UDP 連接埠**1813 年**。
2. 在**驗證伺服器**或**RADIUS 伺服器**，指定 IP 位址或視需求 NAS 的完整的網域名稱 (FQDN)，您 NPS 伺服器。 
3. 在**密碼**或**共用密碼**，輸入穩固密碼。 當您 NAS 設定為在 NPS RADIUS client 時，您將會使用相同的密碼，所以不要忘記。
4. 如果您使用 PEAP 或 EAP 的驗證方法、設定使用 EAP 驗證 NAS。
5. 如果您設定一個 wireless 存取點，在**SSID**，指定服務設定識別碼 \(SSID\)，也就是英數字串做為的網路名稱。 此名稱存取點來廣播 wireless 用戶端，而且會顯示在您 wireless 精確度 \(Wi-Fi\) 熱點的使用者。
6. 如果您設定一個 wireless 存取點，在**802.1 X WPA 和**，讓**IEEE 802.1 X 驗證**如果您想要部署 PEAP MS-CHAP v2、PEAP-TLS 或 EAP-TLS。

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>新增為中 NPS RADIUS Client 的網路存取伺服器

您可以使用此程序，以在 NPS RADIUS client 新增網路存取伺服器。 您可以使用此程序使用主機 NPS RADIUS client 為設定 NAS。

若要完成此程序，您必須成員的**系統管理員**群組。

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>新增為中 NPS RADIUS client 的網路存取伺服器

1. NPS 伺服器，在伺服器管理員中，按一下 [**工具**，然後按一下 [**的網路原則伺服器**。 NPS 主控台開啟。
2. 在 [NPS 主控台中，按兩下 [ **RADIUS 戶端與伺服器]**。 以滑鼠右鍵按一下**RADIUS 戶端**，然後按**新 RADIUS Client**。 
3. 在**新 RADIUS Client**，確認**可讓這個 RADIUS client**核取方塊。
4. 在**新 RADIUS Client**，請在**的易記名稱**，輸入 NAS 顯示的名稱。 在**（IP 或 DNS）的位址**，輸入 NAS IP 位址或完整的網域名稱 (FQDN)。 如果您輸入 FQDN，請按一下**確認**如果您想要確認名稱正確對應至有效的 IP 位址。 
5. 在**新 RADIUS Client**，請在**廠商**，指定 NAS 製造商名稱。 如果您不確定 NAS 製造商名稱，請選取**RADIUS 標準**。
6. 在**新 RADIUS Client**，請在**共用密碼**，執行下列其中一個動作：
    - 確認**手動**已選取，然後在**共用密碼**，輸入長，這也 NAS 上輸入密碼。 共用的密碼中重新輸入**確認共用的密碼**。
    - 選取 [**產生**，然後按一下 [**產生**來自動產生共用的密碼。 儲存 NAS 上產生共用設定密碼，它可以具有 NPS 伺服器通訊。
7. 在**新 RADIUS Client**，請在**的其他選項**，如果您正在使用 EAP 和 PEAP，以外的任何驗證方法，如果您 NAS 支援使用訊息 authenticator 屬性，選取**存取要求郵件必須包含訊息 Authenticator 屬性**。
8. 按一下**[確定]**。 您 NAS 會出現在清單中的設定伺服器 NPS RADIUS 用。

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>在 Windows Server 2016 Datacenter 設定 RADIUS 戶端的 IP 位址

如果您執行 Windows Server 2016 Datacenter，您可以設定中 NPS RADIUS 戶端的 ip。 這可讓您加入大量的 RADIUS 用（例如 wireless 存取點）NPS 主控台一次，而不是將每個 RADIUS client 排列。

如果您在 Windows Server 2016 標準執行 NPS，您無法透過 IP 位址設定 RADIUS 戶端。

使用此程序群組的網路存取伺服器 (Nas) 新增為 RADIUS 戶端設定具有相同的 IP 位址範圍的 IP 位址。

所有的範圍中 RADIUS 用必須使用相同的設定和共用的密碼。

若要完成此程序，您必須成員的**系統管理員**群組。

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>透過 ip RADIUS 戶端設定

1. NPS 伺服器，在伺服器管理員中，按一下 [**工具**，然後按一下 [**的網路原則伺服器**。 NPS 主控台開啟。
2. 在 [NPS 主控台中，按兩下 [ **RADIUS 戶端與伺服器]**。 以滑鼠右鍵按一下**RADIUS 戶端**，然後按**新 RADIUS Client**。
3. 在**新 RADIUS Client**，請在**的易記名稱**，輸入顯示名稱的 Nas 的收藏。
4. 在**位址 \(IP or DNS\)**，輸入 IP 位址範圍 RADIUS 戶端使用無類別間網域路由 \(CIDR\) 記號。 例如，如果 10.10.0.0 Nas 的 IP 位址範圍，輸入**10.10.0.0/16**。
5. 在**新 RADIUS Client**，請在**廠商**，指定 NAS 製造商名稱。 如果您不確定 NAS 製造商名稱，請選取**RADIUS 標準**。
6. 在**新 RADIUS Client**，請在**共用密碼**，執行下列其中一個動作：
    - 確認**手動**已選取，然後在**共用密碼**，輸入長，這也 NAS 上輸入密碼。 共用的密碼中重新輸入**確認共用的密碼**。
    - 選取 [**產生**，然後按一下 [**產生**來自動產生共用的密碼。 儲存 NAS 上產生共用設定密碼，它可以具有 NPS 伺服器通訊。
7. 在**新 RADIUS Client**，請在**的其他選項**，如果您使用任何 EAP 和 PEAP，以外的驗證方法，如果您 Nas 的所有支援使用訊息 authenticator 屬性，選取 [**存取要求郵件必須包含訊息 Authenticator 屬性**。
8. 按一下**[確定]**。 您 Nas 會出現在清單中的設定伺服器 NPS RADIUS 用。

如需詳細資訊，請查看[RADIUS 戶端](nps-radius-clients.md)。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。