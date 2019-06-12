---
title: 領域名稱
description: 本主題提供使用中處理 Windows Server 2016 中的網路原則伺服器連線要求的領域名稱的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 65a272873a60d74efcf417a16fdc84670f5878da
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446999"
---
# <a name="realm-names"></a>領域名稱

>適用於：Windows Server （半年通道），Windows Server 2016


您可以使用本主題針對使用網路原則伺服器的連線要求處理中的領域名稱的概觀。

User-name RADIUS 屬性是字元字串，其中通常包含 使用者帳戶位置和使用者帳戶名稱。 使用者帳戶位置又稱為領域或領域名稱，並與網域，包括 DNS 網域、 Active Directory® 網域，以及 Windows NT 4.0 網域概念同義。 比方說，如果使用者帳戶位於名稱為 example.com 的網域使用者帳戶資料庫，然後 example.com 是領域名稱。

另舉一例，如果 User-name RADIUS 屬性包含使用者名稱user1@example.com、 user1 是使用者帳戶名稱，而 example.com 是領域名稱。 領域名稱可以顯示在 使用者名稱，做為前置詞，或做為尾碼：

- **Example\user1**。 在此範例中，領域名稱**範例**前置詞; 同時它也是作用中的目錄名稱&reg;網域服務\(AD DS\)網域。

- <strong>user1@example.com</strong>. 在此範例中，領域名稱**example.com**是尾碼，以及 DNS 網域名稱或 AD DS 網域的名稱。

您可以使用設計和部署 RADIUS 基礎結構時，連線要求原則中設定的領域名稱，以確保連接要求會路由傳送來自 RADIUS 用戶端，也稱為網路存取伺服器，可以的 RADIUS 伺服器驗證和授權連線要求。

當 NPS 被設定為 RADIUS 伺服器使用預設連線要求原則時，NPS 會處理連線要求中的 NPS 是成員的網域和信任的網域。

若要設定 NPS 做為 RADIUS proxy，並將連線要求轉寄到不受信任的網域，您必須建立新的連線要求原則。 在新的連線要求原則，您必須設定使用者名稱屬性會包含在您想要轉寄的連線要求的 User-name 屬性的領域名稱。 您也必須設定連線要求原則使用的遠端 RADIUS 伺服器群組。 連線要求原則可讓 NPS 在計算哪些連線要求轉送到遠端 RADIUS 伺服器群組，根據 User-name 屬性的領域部分。

## <a name="acquiring-the-realm-name"></a>取得領域名稱

或在使用者電腦上的連接管理員 (CM) 設定檔被設定成自動提供領域名稱的使用者類型密碼型認證 」 在連接期間嘗試時，會提供使用者名稱的領域名稱部分。

您可以指定網路中的使用者在網路連線嘗試期間輸入其認證時，會提供領域名稱。

例如，您可以要求使用者輸入其使用者名稱，包括使用者帳戶名稱和領域名稱，在**使用者名稱**中**Connect**時進行撥號或虛擬私人網路 (VPN) 對話方塊連接。

此外，如果您使用連接管理員管理組件 (CMAK) 建立自訂撥號封裝，您可以協助使用者將領域名稱自動加入至使用者的電腦所安裝的 CM 設定檔中的使用者帳戶名稱。 比方說，您可以在 CM 設定檔中指定領域名稱和使用者名稱語法，讓使用者只需要輸入認證時指定的使用者帳戶名稱。 在此情況下，使用者不需要知道或記得使用者帳戶所在的網域。

在驗證過程中，使用者輸入密碼型認證之後, 的使用者名稱會從存取用戶端網路存取伺服器。 網路存取伺服器會建構連線要求，並納入傳送到 RADIUS proxy 或伺服器的 Access-request 訊息的 User-name RADIUS 屬性中的領域名稱。

如果 NPS 為 RADIUS 伺服器，會將 Access-request 訊息評估的一組已設定的連線要求原則。 連線要求原則的條件可包含使用者名稱屬性的內容的規格。

您可以設定一組特定領域名稱的內送訊息的 User-name 屬性中的連線要求原則。 這可讓您建立路由規則，當 NPS 做為 RADIUS proxy 時，轉寄具有特定領域名稱到一組特定的 RADIUS 伺服器的 RADIUS 訊息。

## <a name="attribute-manipulation-rules"></a>屬性操作規則

RADIUS 訊息處理在本機 （當 NPS 作為 RADIUS 伺服器） 或轉送到另一部 RADIUS 伺服器 （當 NPS 作為 RADIUS proxy） 之前，可以修改訊息中的使用者名稱屬性的屬性操作規則。 您可以設定 User-name 屬性的屬性操作規則選取**使用者名**上**條件**的連線要求原則內容 索引標籤。 NPS 屬性操作規則使用規則運算式語法。

您可以設定 User-name 屬性來變更下列屬性操作規則：

- 領域名稱移除的使用者名稱\(也稱為領域移除\)。 例如，使用者名稱user1@example.com變更為 user1。

- 變更領域名稱而不是它的語法。 例如，使用者名稱user1@example.com變更為user1@wcoast.example.com。

- 變更領域名稱的語法。 例如，將使用者名稱 example\user1 變更為user1@example.com。

根據您設定的屬性操作規則修改 User-name 屬性之後，第一個符合連線要求原則的其他設定會用來判斷是否：

- 在本機 （當 NPS 作為 RADIUS 伺服器），NPS 會處理 Access-request 訊息。

- （當 NPS 作為 RADIUS proxy），NPS 會轉寄訊息到另一部 RADIUS 伺服器。

## <a name="configuring-the-nps-supplied-domain-name"></a>設定 NPS 提供的網域名稱

當使用者名稱不包含網域名稱時，NPS 會提供。 根據預設，NPS 提供的網域名稱會是 NPS 為成員的網域。 您可以指定 NPS 提供的網域名稱，透過下列登錄設定：

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>不當編輯登錄可能會對系統造成嚴重損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

有些非 Microsoft 網路存取伺服器刪除或修改使用者所指定的網域名稱。 因此，網路存取要求是針對預設網域，因此可能不到使用者的帳戶的網域驗證。 若要解決此問題，請設定您的 RADIUS 伺服器，以將使用者名稱變更為正確格式及正確網域名稱。
