---
title: 將 NPS 作為 RADIUS Proxy 規劃
description: 本主題提供有關 Windows Server 2016 中的網路原則伺服器 RADIUS proxy 部署規劃的資訊。
manager: brianlic
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: feb67b757119fd69ea312845d878322f222da50d
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947484"
---
# <a name="plan-nps-as-a-radius-proxy"></a>將 NPS 作為 RADIUS Proxy 規劃

>適用於：Windows Server (半年度管道)、Windows Server 2016

當您部署網路原則伺服器 (NPS) 作為遠端驗證撥入消費者服務 \( RADIUS \) proxy 時，nps 會從 radius 用戶端接收連線要求，例如網路存取伺服器或其他 radius proxy，然後將這些連線要求轉送到執行 NPS 或其他 radius 伺服器的伺服器。 您可以使用這些規劃指導方針來簡化您的 RADIUS 部署。

這些規劃指導方針不包括您想要將 NPS 部署為 RADIUS 伺服器的情況。 當您將 NPS 部署為 RADIUS 伺服器時，NPS 會針對本機網域以及信任本機網域的網域，執行驗證、授權和帳戶處理的連線要求。

在您將 NPS 部署為網路上的 RADIUS proxy 之前，請使用下列指導方針來規劃您的部署。

- 規劃 NPS 設定。

- 規劃 RADIUS 用戶端。

- 規劃遠端 RADIUS 伺服器群組。

- 規劃訊息轉送的屬性操作規則。

- 規劃連接要求原則。

- 規劃 NPS 帳戶處理。

## <a name="plan-nps-configuration"></a>規劃 NPS 設定

當您使用 NPS 做為 RADIUS proxy 時，NPS 會將連線要求轉送到 NPS 或其他 RADIUS 伺服器以進行處理。 因此，NPS proxy 的網域成員資格是不相關的。 Proxy 不需要在 Active Directory Domain Services AD DS 中註冊， \( \) 因為它不需要存取使用者帳戶的撥入屬性。 此外，您不需要在 NPS proxy 上設定網路原則，因為 proxy 不會執行連線要求的授權。 NPS proxy 可以是網域成員，也可以是獨立伺服器，而且沒有網域成員資格。

NPS 必須設定為使用 RADIUS 通訊協定與 RADIUS 用戶端通訊，也稱為網路存取伺服器。 此外，您可以設定 NPS 記錄在事件記錄檔中的事件種類，而且可以輸入伺服器的描述。

### <a name="key-steps"></a>重要步驟

在規劃 NPS proxy 設定期間，您可以使用下列步驟。

- 判斷 NPS proxy 用來從 RADIUS 用戶端接收 RADIUS 訊息，以及將 RADIUS 訊息傳送給遠端 RADIUS 伺服器群組成員的 RADIUS 埠。 預設的使用者資料包協定 (UDP) 埠為1812和1645，適用于 RADIUS 驗證訊息，而 UDP 埠1813和1646用於 RADIUS 帳戶處理訊息。

- 如果 NPS proxy 設定有多張網路介面卡，請判斷您想要允許 RADIUS 流量的介面卡。

- 判斷您希望 NPS 在事件記錄檔中記錄的事件種類。 您可以記錄已拒絕的連接要求、成功的連接要求，或兩者。

- 判斷您是否要部署一個以上的 NPS proxy。 若要提供容錯功能，請使用至少兩個 NPS proxy。 一個 NPS proxy 會用來做為主要 RADIUS proxy，另一個則用來作為備份。 然後，會在兩個 NPS proxy 上設定每個 RADIUS 用戶端。 如果主要 NPS proxy 變得無法使用，則 RADIUS 用戶端會將 Access-Request 訊息傳送至替代的 NPS proxy。

- 規劃用來將一個 NPS proxy 設定複製到其他 NPS proxy 的腳本，以節省管理額外負荷，並防止伺服器設定不正確。 NPS 提供 Netsh 命令，可讓您將所有或部分的 NPS proxy 設定複製到另一個 NPS proxy。 您可以在 Netsh 提示字元手動執行命令。 但是，如果您將命令順序儲存為腳本，您可以在稍後決定變更 proxy 設定時執行腳本。

## <a name="plan-radius-clients"></a>規劃 RADIUS 用戶端

RADIUS 用戶端是網路存取伺服器，例如無線存取點、虛擬私人網路 \( VPN \) 伺服器、802.1 x 功能交換器，以及撥號伺服器。 將連接要求訊息轉送至 RADIUS 伺服器的 RADIUS proxy 也是 RADIUS 用戶端。 NPS 支援所有符合 RADIUS 通訊協定的網路存取伺服器和 RADIUS proxy，如 RFC 2865 「遠端驗證撥入使用者服務 \( RADIUS」 \) 和 rfc 2866 「RADIUS 帳戶處理」中所述。

此外，無線存取點和交換器都必須能夠 802.1 X 驗證。 如果您想要部署可延伸的驗證通訊協定 (EAP) 或受保護的可延伸驗證通訊協定 (PEAP) ，存取點和交換器必須支援使用 EAP。

若要測試無線存取點的 PPP 連線的基本互通性，請將存取點和存取用戶端設定為使用密碼驗證通訊協定 (PAP) 。 使用其他以 PPP 為基礎的驗證通訊協定（例如 PEAP），直到您測試要用於網路存取的通訊協定為止。

### <a name="key-steps"></a>重要步驟

在規劃 RADIUS 用戶端時，您可以使用下列步驟。

- 記錄廠商特定的屬性 (Vsa) 您必須在 NPS 中設定。 如果您的 Nas 需要 Vsa，請記錄 VSA 資訊以供稍後在 NPS 中設定網路原則時使用。

- 記錄 RADIUS 用戶端的 IP 位址和 NPS proxy，以簡化所有裝置的設定。 當您部署 RADIUS 用戶端時，您必須將其設定為使用 RADIUS 通訊協定，並將 NPS proxy IP 位址輸入為驗證服務器。 當您將 NPS 設定為與您的 RADIUS 用戶端通訊時，您必須在 NPS 嵌入式管理單元中輸入 RADIUS 用戶端 IP 位址。

- 在 RADIUS 用戶端和 NPS 嵌入式管理單元中建立共用密碼以進行設定。 在 NPS 中設定 RADIUS 用戶端時，您必須使用共用密碼或密碼來設定 RADIUS 用戶端，您也會在 NPS 嵌入式管理單元中輸入這些用戶端。

## <a name="plan-remote-radius-server-groups"></a>規劃遠端 RADIUS 伺服器群組

當您在 NPS proxy 上設定遠端 RADIUS 伺服器群組時，您會告訴 NPS proxy 從網路存取伺服器和 NPS proxy 或其他 RADIUS proxy 接收的部分或所有連線要求訊息的傳送位置。

您可以使用 NPS 做為 RADIUS proxy，將連線要求轉寄到一或多個遠端 RADIUS 伺服器群組，且每個群組可包含一或多個 RADIUS 伺服器。 當您想要 NPS proxy 將訊息轉送至多個群組時，請為每個群組設定一個連線要求原則。 連線要求原則會包含其他資訊，例如屬性操作規則，告訴 NPS proxy 哪些訊息要傳送至原則中指定的遠端 RADIUS 伺服器群組。

您可以使用適用于 NPS 的 Netsh 命令來設定遠端 RADIUS 伺服器群組，方法是直接在 [遠端 RADIUS 伺服器群組] 底下的 NPS 嵌入式管理單元中設定群組，或執行 [新增連線要求原則] wizard。

### <a name="key-steps"></a>重要步驟

在規劃遠端 RADIUS 伺服器群組時，您可以使用下列步驟。

- 判斷包含您希望 NPS proxy 轉寄連接要求的 RADIUS 伺服器網域。 這些網域包含使用者帳戶，可供透過您部署的 RADIUS 用戶端連線到網路的使用者使用。

- 判斷您是否需要在尚未部署 RADIUS 的網域中新增 RADIUS 伺服器。

- 記錄您想要新增到遠端 RADIUS 伺服器群組之 RADIUS 伺服器的 IP 位址。

- 判斷您需要建立的遠端 RADIUS 伺服器群組數目。 在某些情況下，最好為每個網域建立一個遠端 RADIUS 伺服器群組，然後將該網域的 RADIUS 伺服器新增至該群組。 不過，在某些情況下，您可能會在某個網域中有大量資源，包括具有網域中使用者帳戶的大量使用者、大量的網域控制站，以及大量的 RADIUS 伺服器。 或者，您的網域可能涵蓋了很大的地理區域，導致您在彼此之間的位置有網路存取伺服器和 RADIUS 伺服器。 在這些情況下，您可以在其他情況下，為每個網域建立多個遠端 RADIUS 伺服器群組。

- 在 NPS proxy 和遠端 RADIUS 伺服器上建立共用密碼以進行設定。

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>規劃訊息轉送的屬性操作規則

在連線要求原則中設定的屬性操作規則，可讓您識別想要轉送到特定遠端 RADIUS 伺服器群組的 Access-Request 訊息。

您可以設定 NPS 將所有連線要求轉寄至一個遠端 RADIUS 伺服器群組，而不使用屬性操作規則。

但是，如果您要將連線要求轉送到多個位置，您必須為每個位置建立一個連線要求原則，然後使用您想要轉寄訊息的遠端 RADIUS 伺服器群組來設定原則，以及使用告訴 NPS 要轉寄之訊息的屬性操作規則。

您可以建立下列屬性的規則。

- 呼叫的-工作站識別碼。 網路存取伺服器 (NAS) 的電話號碼。 這個屬性的值是字元字串。 您可以使用模式比對語法指定區碼。

- 呼叫站-識別碼。 來電者使用的電話號碼。 這個屬性的值是字元字串。 您可以使用模式比對語法指定區碼。

- 使用者名稱。 存取用戶端所提供的使用者名稱，以及 NAS 在 RADIUS Access-Request 訊息中包含的使用者名稱。 這個屬性的值是一個字元字串，通常包含領域名稱和使用者帳戶名稱。

若要正確地取代或轉換連接要求之使用者名稱中的領域名稱，您必須在適當的連線要求原則上設定 User-Name 屬性的屬性操作規則。

### <a name="key-steps"></a>重要步驟

在規劃屬性操作規則時，您可以使用下列步驟。

- 規劃從 NAS 透過 proxy 路由傳送至遠端 RADIUS 伺服器的訊息，以確認您有將訊息轉送至 RADIUS 伺服器的邏輯路徑。

- 判斷您想要針對每個連線要求原則使用的一或多個屬性。

- 記錄您計畫針對每個連線要求原則使用的屬性操作規則，並將規則與轉送訊息的遠端 RADIUS 伺服器群組進行比對。

## <a name="plan-connection-request-policies"></a>規劃連接要求原則

當 NPS 做為 RADIUS 伺服器使用時，會針對 NPS 設定預設連線要求原則。 您可以使用其他連線要求原則來定義更明確的條件、建立屬性操作規則，告訴 NPS 哪些訊息要轉寄到遠端 RADIUS 伺服器群組，以及指定 advanced 屬性。 使用 [新增連線要求原則] 嚮導來建立一般或自訂的連線要求原則。

### <a name="key-steps"></a>重要步驟

規劃連接要求原則時，您可以使用下列步驟。

- 在執行 NPS 的每部伺服器上，刪除預設連線要求原則，此原則只會作為 RADIUS proxy。

- 規劃每個原則所需的其他條件和設定，並將這項資訊與遠端 RADIUS 伺服器群組和針對原則規劃的屬性操作規則結合。

- 設計計畫以將一般連線要求原則散發到所有 NPS proxy。 在一個 NPS 上建立多個 NPS proxy 通用的原則，然後使用適用于 NPS 的 Netsh 命令，在所有其他 proxy 上匯入連線要求原則和伺服器設定。

## <a name="plan-nps-accounting"></a>規劃 NPS 帳戶處理

當您將 NPS 設定為 RADIUS proxy 時，您可以將它設定為使用 NPS 格式記錄檔、資料庫相容的格式記錄檔或 NPS SQL Server 記錄來執行 RADIUS 帳戶處理。

您也可以使用下列其中一種記錄格式，將帳戶處理訊息轉送到執行帳戶處理的遠端 RADIUS 伺服器群組。

### <a name="key-steps"></a>重要步驟

在規劃 NPS 帳戶處理期間，您可以使用下列步驟。

- 判斷您是否希望 NPS proxy 執行帳戶處理服務，或是將帳戶處理訊息轉送到遠端 RADIUS 伺服器群組以進行計量。

- 如果您打算將帳戶處理訊息轉送到其他伺服器，請規劃停用本機 NPS proxy 帳戶處理。

- 如果您打算將帳戶處理訊息轉送到其他伺服器，請規劃連接要求原則設定步驟。 如果您停用 NPS proxy 的本機帳戶處理，則您在該 proxy 上設定的每個連線要求原則都必須啟用並正確設定帳戶處理訊息轉送。

- 判斷您想要使用的記錄格式： IAS 格式記錄檔、與資料庫相容的格式記錄檔，或 NPS SQL Server 記錄。

若要設定 NPS 的負載平衡作為 RADIUS proxy，請參閱 [Nps Proxy 伺服器負載平衡](nps-manage-proxy-lb.md)。

