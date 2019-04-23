---
title: 將 NPS 作為 RADIUS Proxy 規劃
description: 本主題提供 Windows Server 2016 中的 < 規劃的網路原則伺服器 RADIUS proxy 部署的相關資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83fbe57ee62480439190dcc53428e02a4f8e6897
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829559"
---
# <a name="plan-nps-as-a-radius-proxy"></a>將 NPS 作為 RADIUS Proxy 規劃

>適用於：Windows Server （半年通道），Windows Server 2016

當您部署網路原則伺服器 (NPS) 設定為遠端驗證撥入使用者服務\(RADIUS\) proxy，NPS 會收到連線要求從 RADIUS 用戶端，例如網路存取伺服器或其他 RADIUS proxy，然後這些將連接要求轉送到執行 NPS 或其他 RADIUS 伺服器的伺服器。 您可以使用下列規劃指導方針，以簡化您的 RADIUS 部署。

這些計劃的指導方針不包含您要部署 NPS 做為 RADIUS 伺服器的情況。 當您部署 NPS 做為 RADIUS 伺服器時，NPS，則會執行驗證、 授權和帳戶處理連接要求的本機網域及信任本機網域的網域。

NPS 做為 RADIUS proxy 部署在網路上之前，使用下列指導方針來規劃您的部署。

- 計劃 NPS 設定。

- 規劃 RADIUS 用戶端。

- 規劃遠端 RADIUS 伺服器群組。

- 規劃訊息轉送的屬性操作規則。

- 計劃連線要求原則。

- 計劃 NPS 帳戶處理。

## <a name="plan-nps-configuration"></a>計劃 NPS 設定

當您使用 NPS 做為 RADIUS proxy 時，NPS 會將連接要求轉送到 NPS 或其他 RADIUS 伺服器進行處理。 因為這個緣故，NPS proxy 的網域成員資格無關。 Proxy 不需要在 Active Directory 網域服務中註冊\(AD DS\)因為不需要存取權的使用者帳戶撥入內容。 此外，您不需要在 NPS proxy 上設定網路原則，因為 proxy 不會執行連線要求授權。 NPS proxy 可以是網域成員，或它可以是任何網域成員資格的獨立伺服器。

NPS 必須設定 RADIUS 用戶端，也稱為 「 網路存取伺服器，使用 RADIUS 通訊協定與通訊。 此外，您可以設定的事件類型，NPS 記錄在事件記錄檔，您可以輸入伺服器的描述。

### <a name="key-steps"></a>主要步驟

在規劃 NPS proxy 設定，您可以使用下列步驟。

- 判斷 NPS proxy 會使用接收自 RADIUS 用戶端的 RADIUS 訊息，並將 RADIUS 訊息傳送到遠端 RADIUS 伺服器群組的成員的 RADIUS 連接埠。 預設使用者資料包通訊協定 (UDP) 連接埠是 1812年與 1645 用於 RADIUS 驗證訊息，而 UDP 連接埠 1813年與 1646 用於 RADIUS 帳戶處理訊息。

- 如果 NPS proxy 設定與多個網路介面卡，判斷哪些您想要允許 RADIUS 流量的配接器。

- 決定您想要 NPS 事件記錄檔中記錄的事件類型。 您可以記錄已拒絕的連線要求，成功的連線要求，或兩者。

- 決定您要部署多個 NPS proxy。 若要提供容錯功能，使用至少兩個 NPS proxy。 一個 NPS proxy 做為主要 RADIUS proxy，另一個則用做為備份。 每個 RADIUS 用戶端則是在這兩個 NPS proxy 設定。 如果無法使用主要的 NPS proxy，RADIUS 用戶端再傳送 Access-request 訊息到替代的 NPS proxy。

- 規劃用來將一個 NPS proxy 設定複製到其他 NPS proxy，若要儲存在系統管理額外負荷，並防止不正確的組態伺服器的指令碼。 NPS 提供的 Netsh 命令可讓您複製全部或部分的匯入另一部 NPS proxy 上的 NPS proxy 設定。 您可以在 Netsh 提示字元中，以手動方式執行命令。 不過，如果您儲存在您的命令順序做為指令碼時，您可以在日後執行指令碼如果您決定要變更您的 proxy 設定。

## <a name="plan-radius-clients"></a>計劃 RADIUS 用戶端

RADIUS 用戶端網路存取伺服器，例如無線存取點、 虛擬私人網路\(VPN\)伺服器、 802.1 X 支援交換器和撥號伺服器。 RADIUS proxy，轉寄連線要求訊息到 RADIUS 伺服器，也是 RADIUS 用戶端。 NPS 支援所有的網路存取伺服器和 RADIUS proxy 與 RADIUS 通訊協定符合 RFC 2865 中所述 「 遠端驗證撥入使用者服務\(RADIUS\)，」 及 RFC 2866「radius 「 RADIUS 計量 」。

此外，無線存取點及交換器必須能夠 802.1x 驗證。 如果您想要部署 「 可延伸驗證通訊協定 (EAP) 或受保護的可延伸驗證通訊協定 (PEAP)，存取點及交換器必須支援使用 EAP。

若要測試的無線存取點的 PPP 連線的基本交互操作性，設定的存取點和存取用戶端使用密碼驗證通訊協定 (PAP)。 使用其他以 PPP 為基礎的驗證通訊協定，例如 PEAP，直到您已經測試過您想要使用的網路存取的項目。

### <a name="key-steps"></a>主要步驟

為 RADIUS 用戶端的規劃，您可以使用下列步驟。

- 您必須在 NPS 中設定文件特定廠商屬性 (Vsa)。 如果您的 Nas 需要 Vsa，記錄的 VSA 資訊供日後使用，當您在 NPS 中設定您的網路原則。

- 文件來簡化的所有裝置組態的 RADIUS 用戶端及 NPS proxy 的 IP 位址。 當您部署您的 RADIUS 用戶端時，您必須設定他們使用 RADIUS 通訊協定中，輸入與驗證伺服器的 NPS proxy IP 位址。 然後當您設定 NPS 與您的 RADIUS 用戶端通訊時，您必須輸入 RADIUS 用戶端 IP 位址 NPS 嵌入式管理單元。

- RADIUS 用戶端與 NPS 嵌入式管理單元，請建立組態的共用的密碼。 使用共用的密碼或密碼，您也將輸入至 NPS 嵌入式管理單元在 NPS 中設定 RADIUS 用戶端時，您必須設定 RADIUS 用戶端。

## <a name="plan-remote-radius-server-groups"></a>規劃遠端 RADIUS 伺服器群組

當您在 NPS proxy 上設定遠端 RADIUS 伺服器群組時，會告訴 NPS proxy 從網路存取伺服器和 NPS proxy 或其他 RADIUS proxy 接收要求訊息傳送部分或所有連線的位置。

您可以使用 NPS RADIUS proxy，以轉送連線要求至其中一個或多個遠端 RADIUS 伺服器群組，以及每個群組可包含一或多個 RADIUS 伺服器。 當您想要將訊息轉送到多個群組的 NPS proxy 時，設定每個群組的一個連線要求原則。 連線要求原則包含其他資訊，例如屬性操作規則，告訴 NPS proxy 傳送到遠端 RADIUS 伺服器群組原則中指定的哪些訊息。

用於 NPS 的 Netsh 命令，直接在遠端 RADIUS 伺服器群組 下 NPS 嵌入式管理單元中設定群組或執行 「 新增連線要求原則精靈 」，您可以設定遠端 RADIUS 伺服器群組。

### <a name="key-steps"></a>主要步驟

規劃遠端 RADIUS 伺服器群組，您可以使用下列步驟。

- 判斷網域來包含您想要 NPS proxy 將連線要求轉送至 RADIUS 伺服器。 這些網域包含連線到您所部署的 RADIUS 用戶端透過網路的使用者的使用者帳戶。

- 決定是否需要新增新的 RADIUS 伺服器 RADIUS 不已經部署所在的網域中。

- 記錄您想要新增到遠端 RADIUS 伺服器群組的 RADIUS 伺服器的 IP 位址。

- 判斷多少遠端的 RADIUS 伺服器群組您需要建立。 在某些情況下，最好是建立網域，每一個遠端 RADIUS 伺服器群組，並再將網域的 RADIUS 伺服器新增至群組。 不過，可能是您有大量的資源在一個網域中，包括大量的使用者帳戶的使用者網域、 網域控制站，大量和大量的 RADIUS 伺服器的情況。 或者，您的網域中可能會涵蓋廣大地理區域，讓您將網路存取伺服器和 RADIUS 伺服器放在彼此之間遙遠的位置。 在這些案例和可能是其他情況下，您可以建立每個網域的多個遠端 RADIUS 伺服器群組。

- 在 NPS proxy 和遠端 RADIUS 伺服器上，請建立組態的共用的密碼。

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>規劃訊息轉送的屬性操作規則

連線要求原則中設定的屬性操作規則，可讓您識別您想要轉送到特定的遠端 RADIUS 伺服器群組的存取要求訊息。

您可以設定 NPS 送給一個遠端 RADIUS 伺服器群組中的所有連線要求，而不需使用屬性操作規則。

如果您有多個您要轉送連線要求的位置，不過，您必須建立一個連線要求原則，針對每個位置，然後設定含有您要將訊息轉送到遠端 RADIUS 伺服器群組的原則，以及使用哪些訊息来轉送告訴 NPS 屬性操作規則。

您可以建立下列屬性的規則。

- 呼叫站台識別碼。 網路存取伺服器 (NAS) 電話號碼。 此屬性的值是字元字串。 您可以使用模式比對語法指定區碼。

- 呼叫站台識別碼。 呼叫端所使用的電話號碼。 此屬性的值是字元字串。 您可以使用模式比對語法指定區碼。

- 使用者名稱。 提供存取用戶端和 RADIUS Access-request 訊息中，NAS 所包含的使用者名稱。 此屬性的值是字元字串，其中通常包含領域名稱與使用者帳戶名稱。

若要正確地取代或轉換中的連線要求的使用者名稱的領域名稱，您必須設定 User-name 屬性的屬性操作規則上適當的連線要求原則。

### <a name="key-steps"></a>主要步驟

在規劃屬性操作規則，您可以使用下列步驟。

- 規劃訊息從 NAS 透過 proxy 路由傳送至遠端 RADIUS 伺服器以確認您擁有用來將訊息轉送至 RADIUS 伺服器的邏輯路徑。

- 決定您想要用於每個連線要求原則的一或多個屬性。

- 文件想要針對每個連線要求原則中，使用屬性操作規則和比對規則，以轉寄訊息的遠端 RADIUS 伺服器群組。

## <a name="plan-connection-request-policies"></a>計劃連線要求原則

預設連線要求原則時它正做為 RADIUS 伺服器為 NPS 中設定。 其他的連線要求原則可用來定義更具體條件、 建立屬性操作規則，告訴 NPS 將轉送到遠端 RADIUS 伺服器群組，並指定進階的屬性的哪些訊息。 若要建立一般或自訂的連線要求原則中使用新的連線要求原則精靈。

### <a name="key-steps"></a>主要步驟

在規劃連線要求原則，您可以使用下列步驟。

- 刪除預設連線要求原則，執行 NPS 的設定，該函式僅做為 RADIUS proxy 的每部伺服器上。

- 規劃其他的條件和所需的每個原則，這項資訊結合遠端 RADIUS 伺服器群組，以及規劃原則的屬性操作規則的設定。

- 設計要散發到所有 NPS proxy 的一般連線要求原則的計劃。 建立單一 NPS，通用於多個 NPS proxy 原則，然後再使用 匯入連線要求原則和伺服器上的設定所有其他 proxy 的 NPS 的 Netsh 命令。

## <a name="plan-nps-accounting"></a>計劃 NPS 帳戶處理

當您設定 NPS 做為 RADIUS proxy 時，您可以設定它來執行 RADIUS 帳戶處理使用 NPS 格式記錄檔、 資料庫相容的格式記錄檔或 NPS SQL 伺服器記錄。

此外，您也可以將帳戶處理訊息轉送至執行帳戶處理使用其中一種記錄格式的遠端 RADIUS 伺服器群組。

### <a name="key-steps"></a>主要步驟

用於 NPS 帳戶處理的規劃，您可以使用下列步驟。

- 決定是否要讓 NPS proxy 來執行帳戶處理服務或帳戶處理訊息轉送到遠端 RADIUS 伺服器群組，用於帳戶處理。

- 若要停用計量，如果您計劃將帳戶處理訊息轉送到其他伺服器的本機 NPS proxy 計劃。

- 如果您計劃將帳戶處理訊息轉送到其他伺服器，請規劃連線要求原則設定步驟。 如果您停用本機 NPS proxy 的計量，您在該 proxy 設定每個連線要求原則必須啟用並正確設定的帳戶處理訊息轉送。

- 決定您想要使用的記錄格式：IAS 格式記錄檔、 資料庫相容的格式記錄檔或 NPS SQL 伺服器記錄。

若要設定 NPS 做為 RADIUS proxy 的負載平衡，請參閱[NPS Proxy 伺服器負載平衡](nps-manage-proxy-lb.md)。

