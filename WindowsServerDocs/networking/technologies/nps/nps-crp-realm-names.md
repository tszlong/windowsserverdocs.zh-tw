---
title: 領域名稱
description: 本主題提供在 Windows Server 2016 中的網路原則伺服器連線要求處理中使用領域名稱的總覽。
manager: brianlic
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e219169ba3132a18c0a76219fa8da96e30902090
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969395"
---
# <a name="realm-names"></a>領域名稱

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，以瞭解在網路原則伺服器連線要求處理中使用領域名稱的總覽。

User-Name RADIUS 屬性是一種字元字串，通常會包含使用者帳戶位置與使用者帳戶名稱。 使用者帳戶位置又稱為領域或領域名稱，與網域概念同義，包括 DNS 網域、Active Directory® 網域以及 Windows NT 4.0 網域。 例如，如果使用者帳戶位於名稱為 example.com 的網域使用者帳戶資料庫中，則領域名稱為 example.com。

在另一個範例中，如果使用者名稱 RADIUS 屬性包含使用者名稱 user1@example.com ，user1 就是使用者帳戶名稱，而 example.com 是領域名稱。 領域名稱可以顯示在使用者名稱的首碼或尾碼：

- **Example\user1**。 在此範例中，領域名稱**範例**是前置詞;而且也是 Active Directory &reg; 網域服務 \( AD DS 網域的名稱 \) 。

- <strong>user1@example.com</strong>. 在此範例中，領域名稱**example.com**是尾碼;而且它可以是 DNS 功能變數名稱或 AD DS 網域的名稱。

您可以使用連線要求原則中設定的領域名稱，同時設計和部署 RADIUS 基礎結構，以確保連線要求從 RADIUS 用戶端 (又稱為網路存取伺服器) 路由傳送到可驗證及授權連線要求的 RADIUS 伺服器。

當 NPS 設定為具有預設連線要求原則的 RADIUS 伺服器時，NPS 會處理 NPS 所屬網域的連線要求，以及信任的網域。

若要將 NPS 設定為 RADIUS Proxy 並將連線要求轉寄到受信任網域，需要建立新的連線要求原則。 在新的連線要求原則中，您必須使用領域名稱來設定 User Name 屬性，領域名稱位於您要轉寄之連線要求的 User-Name 屬性中。 您也必須透過遠端 RADIUS 伺服器群組來設定連線要求原則。 連線要求原則可讓 NPS 根據 User-Name 屬性的領域部分，計算要轉寄到遠端 RADIUS 伺服器群組的連線要求。

## <a name="acquiring-the-realm-name"></a>取得領域名稱

當使用者在連線嘗試期間輸入密碼認證，或是當使用者電腦上的連線管理員 (CM) 設定檔設定成自動提供領域名稱時，就會提供使用者名稱的領域名稱部分。

您可以指定網路使用者在網路連線嘗試期間輸入其認證時提供領域名稱。

例如，您可以要求使用者在進行撥號或虛擬私人網路時，于 [**連線] 對話方塊**的 [**使用者名稱**] 中輸入使用者名稱，包括使用者帳戶名稱和領域名稱 (VPN) 連接。

此外，如果您使用連線管理員系統管理組件 (CMAK) 建立自訂撥號封裝，就可以協助使用者將領域名稱自動加入安裝在使用者電腦上的 CM 設定檔使用者帳戶名稱。 例如，您可以在 CM 設定檔中指定領域名稱與使用者名稱語法，這樣使用者就只需要在輸入認證時指定使用者帳戶名稱。 在此情況下，使用者不需要知道或記得使用者帳戶所在的網域。

在驗證程序期間，當使用者輸入其密碼認證之後，使用者名稱會從存取用戶端傳遞到網路存取伺服器。 網路存取伺服器會建構連線要求，並在傳送到 RADIUS Proxy 或伺服器的 Access-Request 訊息之 User-Name RADIUS 屬性中包括領域名稱。

如果 RADIUS 伺服器是 NPS，則會針對一組設定的連線要求原則來評估存取要求訊息。 連線要求原則的條件可包括 User-Name 屬性的內容規格。

您可以在傳入訊息的 User-Name 屬性中設定領域名稱特定的連線要求原則集合。 這可讓您建立路由規則，以便在 NPS 為 RADIUS Proxy 時，轉寄具有特定領域名稱的 RADIUS 訊息到一組特定的 RADIUS 伺服器。

## <a name="attribute-manipulation-rules"></a>屬性操作規則

在本機處理 RADIUS 訊息 (當 NPS 做為 RADIUS 伺服器使用時) 或是轉寄到其他 RADIUS 伺服器時 (當 NPS 做為 RADIUS Proxy 使用時)，可透過屬性操作規則來修改訊息的 User-Name 屬性。 您可以在連線要求原則內容的 [**條件**] 索引標籤上選取 [**使用者名稱**]，以設定使用者名稱屬性的屬性操作規則。 NPS 屬性操作規則使用一般運算式語法。

您可以設定 User-Name 屬性的屬性操作規則以變更下列項目：

- 從使用者名稱中移除領域名稱， \( 也稱為領域去除 \) 。 例如，使用者名稱 user1@example.com 會變更為 user1。

- 變更領域名稱但不變更語法。 例如，使用者名稱 user1@example.com 會變更為 user1@wcoast.example.com 。

- 變更領域名稱的語法。 例如，使用者名稱 example\user1 會變更為 user1@example.com 。

在根據您設定的屬性操作規則修改 User-Name 屬性之後，第一次符合連線要求原則的其他設定是用以判斷：

- 當 NPS 做為 RADIUS 伺服器) 時，NPS 會 (本機處理存取要求訊息。

- 當 NPS 做為 RADIUS proxy) 使用時，NPS 會將訊息轉送至另一個 RADIUS 伺服器 (。

## <a name="configuring-the-nps-supplied-domain-name"></a>設定 NPS 提供的功能變數名稱

當使用者名稱不包含網域名稱時，NPS 會提供包含網域名稱的使用者名稱。 根據預設，NPS 提供的功能變數名稱是 NPS 所屬的網域。 您可以透過下列登錄設定來指定 NPS 提供的網域名稱：

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
```

> [!CAUTION]
> 不當編輯登錄可能會造成系統嚴重受損。 變更登錄之前，您應該先備份電腦所有的重要資料。

有些非 Microsoft 網路存取伺服器會刪除或修改使用者指定的網域名稱。 因此，會針對預設網域來驗證網路存取要求，但是它可能不是使用者帳戶的網域。 若要解決這個問題，請設定 RADIUS 伺服器以便將使用者名稱變更為正確格式及正確網域名稱。
