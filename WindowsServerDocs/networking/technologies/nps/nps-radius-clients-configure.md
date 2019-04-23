---
title: 設定 RADIUS 用戶端
description: 本主題提供有關設定 Windows Server 2016 中的網路原則伺服器的 RADIUS 用戶端的資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 851bdb0677a17567f81a2c331baad595fe40436a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839489"
---
# <a name="configure-radius-clients"></a>設定 RADIUS 用戶端

>適用於：Windows Server （半年通道），Windows Server 2016

若要設定網路存取伺服器做為 NPS 中的 RADIUS 用戶端，您可以使用本主題。

當您將新的網路存取伺服器\(VPN 伺服器，驗證交換器或撥號伺服器的無線存取點\)到網路時，您必須將伺服器新增為 RADIUS 用戶端在 NPS 中，並接著設定 RADIUS 用戶端通訊使用 NPS。

>[!IMPORTANT]
>用戶端電腦和裝置，例如膝上型電腦、 平板電腦、 手機和其他執行用戶端作業系統的電腦不是 RADIUS 用戶端。 RADIUS 用戶端網路存取伺服器-例如無線存取點、 802.1 X 支援交換器、 虛擬私人網路 (VPN) 伺服器和撥號伺服器-因為 RADIUS 伺服器，例如網路原則與通訊的情況下，他們在使用 RADIUS 通訊協定伺服器\(NPS\)伺服器。

此步驟也是必要的當您的 NPS 設定 NPS proxy 的遠端 RADIUS 伺服器群組的成員。 在此情況下，除了執行步驟，在這項工作在 NPS proxy，您必須執行下列作業：

- 在 NPS proxy 上設定遠端 RADIUS 伺服器群組，其中包含 NPS。
- 在遠端 NPS 中，設定 NPS proxy 做為 RADIUS 用戶端。

若要執行本主題中的程序，您必須至少一個網路存取伺服器\(VPN 伺服器，驗證交換器或撥號伺服器的無線存取點\)或實際安裝在您網路上的 NPS proxy。

## <a name="configure-the-network-access-server"></a>設定網路存取伺服器

若要設定網路存取伺服器使用 nps 使用此程序。 當您部署為 RADIUS 用戶端的網路存取伺服器 (Nas) 時，您必須設定與 Nas 做為用戶端設定的位置 NPSs 通訊的用戶端。

此程序提供您應該用來設定您的 Nas; 的設定有關的一般指導方針如需有關如何設定您要部署在您的網路裝置的特定指示，請參閱 NAS 產品文件。

### <a name="to-configure-the-network-access-server"></a>若要設定網路存取伺服器

1. 在 NAS，在**RADIUS 設定**，選取**RADIUS 驗證**使用者資料包通訊協定 (UDP) 連接埠上**1812年**並**RADIUS 帳戶處理**UDP 連接埠**1813年**。
2. 在 **驗證伺服器**或**RADIUS 伺服器**，指定您的 NPS 依 IP 位址或完整的網域名稱 (FQDN)，根據 NAS 的需求。 
3. 在 **祕密**或是**共用祕密**，輸入強式密碼。 當您在 NPS 中的 RADIUS 用戶端設定的 NAS 時，您將使用相同的密碼，因此請不要忘記。
4. 如果您使用 PEAP 或 EAP 做為驗證方法，設定為使用 EAP 驗證 NAS。
5. 如果您要設定無線存取點，在**SSID**，指定服務組識別元\(SSID\)，這是英數字元的字串，做為網路名稱。 此名稱廣播給無線用戶端存取點，而且是在您無線相容性認證的使用者可以看見\(Wi-fi\)作用點。
6. 如果您要設定無線存取點，在**802.1x 和 WPA**，讓**IEEE 802.1x 驗證**如果您想要部署 PEAP MS-CHAP v2、 PEAP-TLS 或 EAP-TLS。

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>在 NPS 中的 RADIUS 用戶端加入網路存取伺服器

您可以使用此程序，新增在 NPS 中的 RADIUS 用戶端的網路存取伺服器。 若要設定 NAS 的 RADIUS 用戶端，使用 NPS 主控台中，您可以使用此程序。

若要完成此程序，您必須是 **Administrators** 群組的成員。

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>若要新增網路存取伺服器做為 NPS 中的 RADIUS 用戶端

1. 在 NPS 中，在 伺服器管理員 中，按一下 **工具**，然後按一下**網路原則伺服器**。 NPS 主控台隨即開啟。
2. 在 NPS 主控台中，按兩下**RADIUS 用戶端和伺服器**。 以滑鼠右鍵按一下**RADIUS 用戶端**，然後按一下**新增 RADIUS 用戶端**。 
3. 在 **新增 RADIUS 用戶端**，確認**啟用此 RADIUS 用戶端**選取核取方塊。
4. 在 **新增 RADIUS 用戶端**，請在**易記名稱**，輸入 NAS 的顯示名稱。 在 **位址 （IP 或 DNS）**，輸入的 NAS IP 位址或完整的網域名稱 (FQDN)。 如果您輸入的 FQDN，請按一下**確認**如果您想要確認名稱正確，且對應至有效的 IP 位址。 
5. 在 **新增 RADIUS 用戶端**，請在**廠商**，指定 NAS 製造商名稱。 如果您不確定的 NAS 製造商名稱，請選取**RADIUS 標準**。
6. 在 **新增 RADIUS 用戶端**，請在**共用祕密**，執行下列其中一項：
    - 請確認**手動**已選取，然後在**共用祕密**，輸入在 NAS 也將輸入強式密碼。 重新輸入中的共用的密碼**確認共用的密碼**。
    - 選取**Generate**，然後按一下**產生**來自動產生 共用的密碼。 儲存組態的產生共用的密碼在 NAS，以便它可以與 NPS 進行通訊。
7. 在 **新增 RADIUS 用戶端**，請在**其他選項**，如果您使用非 EAP 與 PEAP，任何的驗證方法，且如果您的 NAS 支援訊息驗證者屬性中的，選取**存取要求訊息必須包含訊息驗證者屬性**。
8. 按一下 [確定] 。 您的 NAS 會出現在 NPS 上設定的 RADIUS 用戶端的清單。

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>在 Windows Server 2016 資料中心中設定 RADIUS 用戶端的 IP 位址範圍

如果您執行 Windows Server 2016 Datacenter，您可以在 NPS 中設定 RADIUS 用戶端的 IP 位址範圍。 這可讓您將新增大量 RADIUS 用戶端 （例如無線存取點） 至 NPS 主控台中一次，而不是個別加入每個 RADIUS 用戶端。

如果您在 Windows Server 2016 Standard 執行 NPS，您無法設定 RADIUS 用戶端的 IP 位址範圍。

您可以使用此程序新增為 RADIUS 用戶端設定與來自相同的 IP 位址範圍的 IP 位址的網路存取伺服器 (Nas) 的群組。

所有範圍中的 RADIUS 用戶端必須使用相同的組態和共用的密碼。

若要完成此程序，您必須是 **Administrators** 群組的成員。

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>若要設定 RADIUS 用戶端的 IP 位址範圍

1. 在 NPS 中，在 伺服器管理員 中，按一下 **工具**，然後按一下**網路原則伺服器**。 NPS 主控台隨即開啟。
2. 在 NPS 主控台中，按兩下**RADIUS 用戶端和伺服器**。 以滑鼠右鍵按一下**RADIUS 用戶端**，然後按一下**新增 RADIUS 用戶端**。
3. 在 **新增 RADIUS 用戶端**，請在**易記名稱**，輸入集合的 Nas 的顯示名稱。
4. 在 **地址\(IP 或 DNS\)**，輸入 RADIUS 用戶端的 IP 位址範圍，使用無類別網域間路由\(CIDR\)標記法。 例如，如果 10.10.0.0 Nas 的 IP 位址範圍，鍵入**10.10.0.0/16**。
5. 在 **新增 RADIUS 用戶端**，請在**廠商**，指定 NAS 製造商名稱。 如果您不確定的 NAS 製造商名稱，請選取**RADIUS 標準**。
6. 在 **新增 RADIUS 用戶端**，請在**共用祕密**，執行下列其中一項：
    - 請確認**手動**已選取，然後在**共用祕密**，輸入在 NAS 也將輸入強式密碼。 重新輸入中的共用的密碼**確認共用的密碼**。
    - 選取**Generate**，然後按一下**產生**來自動產生 共用的密碼。 儲存組態的產生共用的密碼在 NAS，以便它可以與 NPS 進行通訊。
7. 在 **新增 RADIUS 用戶端**，請在**其他選項**，如果您使用非 EAP 與 PEAP，任何的驗證方法，且如果您的 Nas 的所有支援訊息驗證者屬性中的，選取**存取要求訊息必須包含訊息驗證者屬性**。
8. 按一下 [確定] 。 您的 Nas 會出現在 NPS 上設定的 RADIUS 用戶端的清單。

如需詳細資訊，請參閱 < [RADIUS 用戶端](nps-radius-clients.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。