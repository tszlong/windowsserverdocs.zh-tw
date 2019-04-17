---
title: 規劃 NPS RADIUS proxy 為
description: 本主題提供的網路原則伺服器 RADIUS proxy 部署計劃，在 Windows Server 2016 中相關資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 966e555ebcac6c7daf4a26b322f0d29f023f8539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="plan-nps-as-a-radius-proxy"></a>規劃 NPS RADIUS proxy 為

>適用於：Windows Server（以每年次管道）、Windows Server 2016

當您要部署的網路原則伺服器 (NPS) 為撥號使用者服務遠端驗證 \(RADIUS\) proxy 時，NPS 接收連接要求從 RADIUS，例如網路存取伺服器或其他 RADIUS proxy，，然後再轉寄執行 NPS 或其他 RADIUS 伺服器伺服器這些連接要求。 您可以使用下列計劃的指導方針操作來簡化 RADIUS 部署。

這些計劃的指導方針不包含您要部署 NPS RADIUS 伺服器的環境。 當您部署 NPS RADIUS 伺服器時、 NPS 執行驗證、 授權及計量本機網域和信任本機網域的連接要求。

您部署 NPS RADIUS proxy 為您網路上之前，請使用下列指導方針計劃部署。

- 規劃伺服器 NPS 的設定。

- 規劃 RADIUS 戶端。

- 規劃遠端 RADIUS 伺服器群組。

- 規劃郵件轉寄屬性操作規則。

- 規劃連接要求原則。

- 規劃 NPS 計量。

## <a name="plan-nps-server-configuration"></a>計劃 NPS 伺服器設定

當您使用 NPS RADIUS proxy 時，NPS 轉送 NPS 伺服器或處理其他 RADIUS 伺服器連接要求。 因此，NPS proxy 的網域成員資格無關。 不需要 proxy 登記在 Active Directory Domain Services \(AD DS\) 因為就不需要存取權的使用者帳號撥號屬性。 此外，您不需要的網路原則設定 NPS proxy 上，因為 proxy 不會執行連接要求的授權。 NPS proxy 可以網域隸屬或不網域成員資格獨立伺服器。

必須設定 NPS RADIUS 戶端，也透過使用 RADIUS 通訊協定稱為網路存取伺服器的通訊。 此外，您可以設定的事件類型該 NPS 記錄事件登入，您可以輸入伺服器的描述。

### <a name="key-steps"></a>步驟鍵

規劃 NPS proxy 設定，您可以使用下列步驟。

- 判斷 RADIUS 用連接埠的 NPS proxy RADIUS 從接收 RADIUS 訊息和傳送簡訊 RADIUS 遠端 RADIUS 伺服器群組成員。 預設的使用者資料流通訊協定 (UDP) 連接埠的 1812年和 RADIUS 驗證訊息與 UDP 連接埠 1813年和 RADIUS 計量郵件 1646年 1645年。

- 如果 NPS proxy 設定多個網路介面卡，來判斷要允許 RADIUS 流量的介面卡。

- 確定您想要在事件登入記錄 NPS 活動的類型。 您可以登入拒絕的連接要求、 要求連接成功，或兩者。

- 判斷是否部署多個 NPS proxy。 若要提供容錯，使用兩個以上 NPS proxy。 一個 NPS proxy 做為主要 RADIUS proxy，並用另做為備份。 每個 RADIUS client 再是在兩個 NPS proxy 設定。 如果主要 NPS proxy 無法使用，RADIUS 戶端再傳送存取要求訊息給其他 NPS proxy。

- 規劃用一個 NPS proxy 設定複製到其他 NPS proxy 儲存在 [管理費用，並使的伺服器設定正確的指令碼。 NPS 提供可讓您要匯入到另一個 NPS proxy NPS proxy 設定的部分或全部複製 Netsh 命令。 您可以在 Netsh 命令提示字元中手動執行的命令。 不過，如果您儲存您命令順序做為指令碼時，您可以日後執行指令碼如果您要變更您的 proxy 設定。

## <a name="plan-radius-clients"></a>RADIUS 戶端計劃

RADIUS 戶端的網路存取伺服器 wireless 存取點，例如 virtual 私人網路 \(VPN\) 伺服器 802.1 X 處理能力的參數和撥號伺服器。 RADIUS proxy，向前連接 RADIUS 伺服器要求訊息，也有 RADIUS 戶端。 NPS 支援所有的網路存取伺服器以及 RADIUS 通訊協定，以符合 RADIUS proxy RFC 2865 中所述 」 遠端驗證入的使用者服務 \(RADIUS\)，「 RFC 2866，和 「 RADIUS 計量 」。

此外，同時 wireless 存取點和參數必須 802.1 X 驗證的功能。 如果您想要部署延伸驗證通訊協定 (EAP) 或保護延伸驗證通訊協定 (PEAP)，存取點和參數必須支援 EAP 使用。

若要測試交互的 PPP 連接 wireless 存取點的基本操作，設定存取點及存取 client 使用密碼驗證通訊協定 (PAP)。 使用其他 PPP 驗證通訊協定，PEAP，例如，直到您想要使用的網路存取權的測試結果。

### <a name="key-steps"></a>步驟鍵

規劃 RADIUS 戶端，您可以使用下列步驟。

- 您必須設定 NPS 文件特定廠商屬性 (Vsa)。 如果您 Nas 需要 Vsa，當您設定您的網路原則 NPS 在登入 VSA 資訊以供之後使用。

- 文件 RADIUS 戶端和您 NPS proxy 簡化的所有裝置的組態的 IP 位址。 當部署 RADIUS 戶端時，您必須他們使用 RADIUS 通訊協定，以驗證伺服器輸入 NPS proxy IP 位址設定。 當您設定 NPS RADIUS 戶端的通訊，您必須到 NPS 嵌入式管理單元輸入 RADIUS client IP 位址。

- 在 RADIUS 戶端和 NPS 嵌入式管理單元，建立共用機密資料的設定。 您必須設定 RADIUS 戶端共用的密碼，或您也會在設定 NPS RADIUS 戶端時 NPS 嵌入式管理單元輸入的密碼。

## <a name="plan-remote-radius-server-groups"></a>規劃遠端 RADIUS 伺服器群組

當您在 NPS proxy 設定遠端 RADIUS 伺服器群組時，您會告訴 NPS proxy 將部分或所有連接要求從網路存取伺服器或其他 RADIUS proxy NPS proxy 收到的簡訊的位置。

您可以使用 NPS RADIUS proxy 連接轉送給要求一或多個遠端 RADIUS 伺服器群組，與每個群組可以包含一或多個 RADIUS 伺服器。 當您想 NPS proxy 轉寄簡訊多個群組時，設定一個連接要求原則每個群組。 連接要求原則包含額外的資訊，例如屬性操作規則，告訴 NPS proxy 傳送到遠端 RADIUS 伺服器群組原則中指定的訊息。

使用 Netsh 命令 NPS、 直接 NPS 嵌入式管理單元遠端 RADIUS 伺服器群組] 下設定群組或執行全新連接要求原則精靈，您可以設定遠端 RADIUS 伺服器群組。

### <a name="key-steps"></a>步驟鍵

規劃遠端 RADIUS 伺服器群組，您可以使用下列步驟。

- 判斷包含您想要轉送連接要求 proxy NPS RADIUS 伺服器的網域。 這些網域包含帳號連接到您的部署 RADIUS 戶端網路的使用者。

- 判斷您要新增位置 RADIUS 不已經部署的網域中的新 RADIUS 伺服器。

- 文件您想要新增到遠端 RADIUS 伺服器群組 RADIUS 伺服器的 IP 位址。

- 判斷多少遠端的 RADIUS 伺服器群組您必須建立。 有時候，最好建立網域每一個遠端 RADIUS 伺服器群組，然後將 RADIUS 伺服器的網域新增到群組。 不過，可能是您有大量的資源，包括大量的使用者使用帳號網域，大量的網域控制站和大量 RADIUS 伺服器一個網域中的案例。 或在您的網域可能實體鍵盤保護蓋大的地理區域，導致網路存取伺服器以及 RADIUS 伺服器彼此的位置。 您可以在這些，可能有時建立網域每多遠端 RADIUS 伺服器群組。

- 建立設定共用的密碼，在 NPS proxy 和遠端 RADIUS 伺服器上。

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>規劃郵件轉寄屬性操作規則

屬性操作規則連接要求原則設定可讓您找出您想要到特定遠端 RADIUS 伺服器群組轉寄要求存取訊息。

您可以設定 NPS 轉寄給一個遠端 RADIUS 伺服器群組的所有連接要求不用屬性操作規則。

如果您有多個位置，您要轉送連接要求，但是，您必須建立連接要求原則為每個位置，然後設定原則，使用遠端 RADIUS 伺服器群組到您想要轉寄簡訊，以及與定期 NPS 轉寄屬性操作規則。

您可以建立規則下列屬性。

- 呼叫基座 id。 電話號碼的網路存取伺服器 (NAS)。 此屬性的值為字串。 您可以使用模式符合語法指定區域驗證碼。

- Id 基座通話。 使用的電話號碼的來電者。 此屬性的值為字串。 您可以使用模式符合語法指定區域驗證碼。

- 使用者名稱。 存取 client，提供及 NAS RADIUS 存取要求訊息中所包含的使用者名稱。 此屬性的值為字元字串，通常會包含領域名稱與 account 使用者名稱。

正確取代或轉換領域名稱連接要求的使用者名稱，您必須在適當地連接要求原則設定屬性操作規則的使用者名稱屬性。

### <a name="key-steps"></a>步驟鍵

規劃屬性操作規則，您可以使用下列步驟。

- 規劃訊息路由透過 proxy NAS 從遠端 RADIUS 伺服器以確認您擁有轉寄簡訊 RADIUS 伺服器的邏輯路徑。

- 判斷您想要使用的每個連接要求原則的一個或多個屬性。

- 想要使用的每個連接要求原則，屬性操作規則文件和符合規則遠端 RADIUS 伺服器群組的轉寄簡訊。

## <a name="plan-connection-request-policies"></a>規劃連接要求原則

預設連接要求原則被設定為 NPS RADIUS 伺服器為使用時。 連接其他要求原則可用來定義特定更多的條件，請定期 NPS RADIUS 伺服器群組遠端，轉送給並指定進階的屬性的操作規則建立屬性。 使用新的連接要求原則精靈建立一般] 或 [自訂連接要求原則。

### <a name="key-steps"></a>步驟鍵

規劃連接要求原則，您可以使用下列步驟。

- Delete 預設連接要求原則執行 NPS RADIUS proxy 僅作為運作的每個伺服器上。

- 規劃其他條件和設定所需的每個原則，與遠端 RADIUS 伺服器群組原則的計劃屬性操作規則組合這項資訊。

- 設計通用連接要求原則所有 NPS proxy 將計畫。 原則常見多個 NPS proxy 伺服器上建立一個 NPS，並使用 Netsh 命令 NPS 匯入所有其他 proxy 連接要求原則和伺服器設定。

## <a name="plan-nps-accounting"></a>NPS 計量計劃

當您設定 NPS RADIUS proxy 為時，您可以將它執行 RADIUS 計量使用 NPS 格式登入檔案，資料庫相容的格式登入或 NPS SQL Server 登設定。

您也可以使用其中一種登入格式執行計量遠端 RADIUS 伺服器群組轉寄計量訊息。

### <a name="key-steps"></a>步驟鍵

規劃 NPS 計量，您可以使用下列步驟。

- 判斷您是否想要 NPS proxy 執行計量服務，或將計量郵件轉寄給計量遠端 RADIUS 伺服器群組。

- 若要停用本機 NPS proxy 計量如果您想要將其他伺服器計量郵件轉寄計劃。

- 如果您想要將其他伺服器計量郵件轉寄，計劃連接要求原則設定步驟。 如果您停用本機計量 NPS proxy，每個您設定在 [proxy 連接要求原則必須計量郵件轉寄功能，並設定正確。

- 判斷您想要使用的登入格式： IAS 格式登入檔案，資料庫相容的格式登入或 NPS SQL Server 登入。

若要設定的負載平衡 NPS RADIUS proxy 為，查看[NPS Proxy 伺服器負載平衡](nps-manage-proxy-lb.md)。

