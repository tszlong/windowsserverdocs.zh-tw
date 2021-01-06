---
title: 設定 RADIUS 用戶端
description: 本主題提供在 Windows Server 2016 中為網路原則伺服器設定 RADIUS 用戶端的相關資訊。
manager: brianlic
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 5f7be2a73fe5bcecad7bee870a96627cacf36a07
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947464"
---
# <a name="configure-radius-clients"></a>設定 RADIUS 用戶端

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，將網路存取伺服器設定為 NPS 中的 RADIUS 用戶端。

當您將新的網路存取伺服器 \( VPN 伺服器、無線存取點、驗證交換器或撥號伺服器新增 \) 至您的網路時，您必須將伺服器新增為 nps 中的 radius 用戶端，然後設定 RADIUS 用戶端與 nps 進行通訊。

>[!IMPORTANT]
>用戶端電腦和裝置，例如膝上型電腦、平板電腦、電話以及執行用戶端作業系統的其他電腦，都不是 RADIUS 用戶端。 RADIUS 用戶端是網路存取伺服器，例如無線存取點、802.1 X 可用交換器、虛擬私人網路 (VPN) 伺服器和撥號伺服器-因為它們使用 RADIUS 通訊協定與 RADIUS 伺服器通訊，例如網路原則伺服器 \( NPS \) 伺服器。

當 NPS 是 NPS proxy 上設定的遠端 RADIUS 伺服器群組的成員時，也需要此步驟。 在此情況下，除了在 NPS proxy 上執行此工作中的步驟之外，您還必須執行下列動作：

- 在 NPS proxy 上，設定包含 NPS 的遠端 RADIUS 伺服器群組。
- 在遠端 NPS 上，將 NPS proxy 設定為 RADIUS 用戶端。

若要執行本主題中的程式，您的網路上必須至少有一個網路存取伺服器 \( VPN 伺服器、無線存取點、驗證交換器或撥號伺服器 \) 或 NPS proxy。

## <a name="configure-the-network-access-server"></a>設定網路存取伺服器

您可以使用此程式來設定網路存取伺服器，以搭配 NPS 使用。 當您將網路存取伺服器部署 (Nas) 作為 RADIUS 用戶端時，您必須將用戶端設定為與 Nas 設定為用戶端的 NPSs 通訊。

此程式提供有關您應該用來設定 Nas 之設定的一般指導方針;如需有關如何設定您要在網路上部署之裝置的特定指示，請參閱您的 NAS 產品檔。

### <a name="to-configure-the-network-access-server"></a>設定網路存取伺服器

1. 在 NAS 的 [ **radius 設定**] 中，選取 [使用者資料包協定上的 **radius 驗證**] (udp) 埠 **1812** 和 udp 埠 **1813** 上的 **radius 帳戶** 處理。
2. 在 [ **驗證服務器** ] 或 [ **RADIUS 伺服器**] 中，根據 NAS 的需求，根據 [IP 位址] 或 [完整功能變數名稱] (FQDN) 指定 NPS。
3. 在 [ **秘密** 或 **共用** 密碼] 中，輸入強式密碼。 當您將 NAS 設定為 NPS 中的 RADIUS 用戶端時，您將會使用相同的密碼，因此請不要忘記密碼。
4. 如果您使用 PEAP 或 EAP 作為驗證方法，請將 NAS 設定為使用 EAP 驗證。
5. 如果您要設定無線存取點，請在 [ **SSID**] 中指定服務組識別元 \( ssid，也 \) 就是作為網路名稱的英數位元字串。 此名稱是由存取點廣播到無線用戶端，而且在您的無線精確 Wi-fi 熱點上，使用者可以看到此名稱 \( \) 。
6. 如果您要設定無線存取點，在 **802.1 x 和 WPA** 中，如果您想要部署 PEAP-ms-chap V2、PEAP-TLS 或 eap-tls，請啟用 **IEEE 802.1 x 驗證** 。

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>新增網路存取伺服器做為 NPS 中的 RADIUS 用戶端

您可以使用此程式，將網路存取伺服器新增為 NPS 中的 RADIUS 用戶端。 您可以使用此程式，使用 NPS 主控台將 NAS 設定為 RADIUS 用戶端。

若要完成此程序，您必須是 **Administrators** 群組的成員。

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>新增網路存取伺服器做為 NPS 中的 RADIUS 用戶端

1. 在 NPS 的伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **網路原則伺服器**]。 NPS 主控台隨即開啟。
2. 在 NPS 主控台中，按兩下 [ **RADIUS 用戶端和伺服器**]。 在 [ **Radius 客戶** 端] 上按一下滑鼠右鍵，然後按一下 [ **新增 radius 用戶端**]。
3. 在 [ **新增 Radius 用戶端**] 中，確認已選取 [ **啟用此 radius 用戶端** ] 核取方塊。
4. 在 [ **新的 RADIUS 用戶端**] 的 [ **易記名稱**] 中，輸入 NAS 的顯示名稱。 在 [ **IP 或 DNS) 的位址 (** 中，輸入 NAS IP 位址或完整功能變數名稱 (FQDN) 。 如果您輸入 FQDN，請按一下 [ **驗證** ]，確認是否要確認名稱是否正確，並對應至有效的 IP 位址。
5. 在 [ **新的 RADIUS 用戶端**] 的 [ **廠商**] 中，指定 NAS 製造商名稱。 如果您不確定 NAS 製造商名稱，請選取 [ **RADIUS 標準**]。
6. 在 [ **新的 RADIUS 用戶端**] 的 [ **共用機密**] 中，執行下列其中一項：
    - 確定已選取 [ **手動** ]，然後在 [ **共用密碼**] 中輸入在 NAS 上也輸入的強式密碼。 在 [ **確認共用密碼**] 中重新輸入共用密碼。
    - 選取 [ **產生**]，然後按一下 [ **產生** ] 以自動產生共用機密。 將產生的共用秘密儲存在 NAS 上，使其可以與 NPS 通訊。
7. 在 **新的 RADIUS 用戶端** 的 [ **其他選項**] 中，如果您使用 EAP 和 PEAP 以外的任何驗證方法，而且如果您的 NAS 支援使用 [訊息驗證者] 屬性，請選取 **[存取要求訊息必須包含訊息驗證** 者] 屬性。
8. 按一下 [確定]。 您的 NAS 會出現在 NPS 上設定的 RADIUS 用戶端清單中。

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>在 Windows Server 2016 Datacenter 中依 IP 位址範圍設定 RADIUS 用戶端

如果您執行的是 Windows Server 2016 Datacenter，您可以依 IP 位址範圍在 NPS 中設定 RADIUS 用戶端。 這可讓您將大量的 RADIUS (用戶端（例如無線存取點）) 一次新增至 NPS 主控台，而不是個別新增每個 RADIUS 用戶端。

如果您在 Windows Server 2016 Standard 上執行 NPS，則無法依 IP 位址範圍設定 RADIUS 用戶端。

您可以使用此程式，將網路存取伺服器的群組新增 (的 Nas) 作為 RADIUS 用戶端，這些用戶端全都是以相同 IP 位址範圍中的 IP 位址設定。

範圍中的所有 RADIUS 用戶端都必須使用相同的設定和共用密碼。

若要完成此程序，您必須是 **Administrators** 群組的成員。

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>依 IP 位址範圍設定 RADIUS 用戶端

1. 在 NPS 的伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **網路原則伺服器**]。 NPS 主控台隨即開啟。
2. 在 NPS 主控台中，按兩下 [ **RADIUS 用戶端和伺服器**]。 在 [ **Radius 客戶** 端] 上按一下滑鼠右鍵，然後按一下 [ **新增 radius 用戶端**]。
3. 在 [ **新的 RADIUS 用戶端**] 的 [ **易記名稱**] 中，輸入 nas 集合的顯示名稱。
4. 在 [**位址 \( IP 或 \) DNS**] 中，使用無類別 Inter-Domain 路由 \( CIDR 標記法，輸入 RADIUS 用戶端的 IP 位址範圍 \) 。 例如，如果 Nas 的 IP 位址範圍是10.10.0.0，請輸入 **10.10.0.0/16**。
5. 在 [ **新的 RADIUS 用戶端**] 的 [ **廠商**] 中，指定 NAS 製造商名稱。 如果您不確定 NAS 製造商名稱，請選取 [ **RADIUS 標準**]。
6. 在 [ **新的 RADIUS 用戶端**] 的 [ **共用機密**] 中，執行下列其中一項：
    - 確定已選取 [ **手動** ]，然後在 [ **共用密碼**] 中輸入在 NAS 上也輸入的強式密碼。 在 [ **確認共用密碼**] 中重新輸入共用密碼。
    - 選取 [ **產生**]，然後按一下 [ **產生** ] 以自動產生共用機密。 將產生的共用秘密儲存在 NAS 上，使其可以與 NPS 通訊。
7. 在 **新的 RADIUS 用戶端** 的 [ **其他選項**] 中，如果您使用 EAP 和 PEAP 以外的任何驗證方法，而且您的所有 nas 都支援使用 [訊息驗證者] 屬性，則選取 **[存取要求訊息必須包含訊息驗證** 者] 屬性。
8. 按一下 [確定]。 您的 Nas 會出現在 NPS 上設定的 RADIUS 用戶端清單中。

如需詳細資訊，請參閱 [RADIUS 用戶端](nps-radius-clients.md)。

如需有關 NPS 的詳細資訊，請參閱 [網路原則伺服器 (nps) ](nps-top.md)。