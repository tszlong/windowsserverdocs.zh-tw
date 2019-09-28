---
title: 領域名稱
description: 本主題提供在 Windows Server 2016 中的網路原則伺服器連線要求處理中使用領域名稱的總覽。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f9c611b793df36c2e588b2fa099df4e5382194c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405464"
---
# <a name="realm-names"></a>領域名稱

>適用於：Windows Server (半年度管道)、Windows Server 2016


您可以使用本主題，以瞭解在網路原則伺服器連線要求處理中使用領域名稱的總覽。

使用者名稱 RADIUS 屬性是一個字元字串，通常包含使用者帳戶位置和使用者帳戶名稱。 使用者帳戶位置也稱為「領域」或「領域名稱」，與網域的概念同義，包括 DNS 網域、Active Directory®網域和 Windows NT 4.0 網域。 例如，如果使用者帳戶位於名為 example.com 之網域的使用者帳戶資料庫中，則 example.com 是領域名稱。

在另一個範例中，如果使用者名稱 RADIUS 屬性包含 user1@example.com 的使用者名稱，user1 就是使用者帳戶名稱，而 example.com 是領域名稱。 領域名稱可以在使用者名稱中以前置詞或尾碼形式呈現：

- **Example\user1**。 在此範例中，領域名稱**範例**是前置詞;此外，它也是 Active Directory @ no__t-1 網域服務 \(AD DS @ no__t-3 網域的名稱。

- <strong>user1@example.com</strong>。 在此範例中，領域名稱**example.com**是尾碼;而且它可以是 DNS 功能變數名稱或 AD DS 網域的名稱。

您可以使用連線要求原則中設定的領域名稱，同時設計和部署 RADIUS 基礎結構，以確保從 RADIUS 用戶端（也稱為網路存取伺服器）將連線要求路由傳送至能夠驗證和授權連接要求。

當 NPS 設定為具有預設連線要求原則的 RADIUS 伺服器時，NPS 會處理 NPS 所屬網域的連線要求，以及信任的網域。

若要將 NPS 設定為 RADIUS proxy，並將連線要求轉送到不受信任的網域，您必須建立新的連線要求原則。 在 [新增連線要求原則] 中，您必須將 [使用者名稱] 屬性設定為 [領域名稱]，該名稱會包含在您想要轉送之連線要求的 [使用者名稱] 屬性中。 您也必須使用遠端 RADIUS 伺服器群組來設定連線要求原則。 連線要求原則可讓 NPS 根據使用者名稱屬性的領域部分，計算要轉送到遠端 RADIUS 伺服器群組的連接要求。

## <a name="acquiring-the-realm-name"></a>取得領域名稱

當使用者在連線嘗試期間輸入密碼型認證，或使用者電腦上的連線管理員（CM）設定檔設定為自動提供領域名稱時，會提供使用者名稱的領域名稱部分。

您可以指定網路使用者在網路連線嘗試期間輸入其認證時，提供其領域名稱。

例如，您可以要求使用者在進行撥號或虛擬私人網路（VPN）連線時，于 [**連線] 對話方塊**的 [**使用者名稱**] 中輸入使用者名稱，包括使用者帳戶名稱和領域名稱。

此外，如果您使用連線管理員系統管理元件（CMAK）來建立自訂撥號封裝，您可以將領域名稱自動新增到安裝在使用者電腦上 CM 設定檔中的使用者帳戶名稱，以協助使用者。 例如，您可以在 CM 設定檔中指定領域名稱和使用者名稱語法，讓使用者只需要在輸入認證時指定使用者帳戶名稱。 在此情況下，使用者不需要知道或記住其使用者帳戶所在的網域。

在驗證過程中，使用者輸入其密碼型認證之後，使用者名稱會從存取用戶端傳遞至網路存取伺服器。 網路存取伺服器會在傳送至 RADIUS proxy 或伺服器的存取要求訊息中，建立連線要求並包含使用者名稱 RADIUS 屬性內的領域名稱。

如果 RADIUS 伺服器是 NPS，則會針對一組設定的連線要求原則來評估存取要求訊息。 連接要求原則的條件可以包含使用者名稱屬性的內容規格。

您可以在傳入訊息的 [使用者名稱] 屬性內，設定領域名稱特有的一組連線要求原則。 這可讓您建立路由規則，以便在 NPS 作為 RADIUS proxy 使用時，將具有特定領域名稱的 RADIUS 訊息轉送至一組特定的 RADIUS 伺服器。

## <a name="attribute-manipulation-rules"></a>屬性操作規則

在本機處理 RADIUS 訊息（當 NPS 做為 RADIUS 伺服器使用時），或轉送至另一個 RADIUS 伺服器（當 NPS 做為 RADIUS proxy 使用時），訊息中的使用者名稱屬性可以由屬性操作規則進行修改。 您可以在連線要求原則內容的 [**條件**] 索引標籤上選取 [**使用者名稱**]，以設定使用者名稱屬性的屬性操作規則。 NPS 屬性操作規則使用正則運算式語法。

您可以設定使用者名稱屬性的屬性操作規則，以變更下列各項：

- 從使用者名稱中移除領域名稱 \(also，稱為領域去除 @ no__t-1。 例如，user1@example.com 的使用者名稱會變更為 user1。

- 變更領域名稱，而不是其語法。 例如，user1@example.com 的使用者名稱會變更為 user1@wcoast.example.com。

- 變更領域名稱的語法。 例如，使用者名稱 example\user1 會變更為 user1@example.com。

根據您設定的屬性操作規則修改使用者名稱屬性之後，會使用第一個相符連接要求原則的其他設定來判斷是否：

- NPS 會在本機處理存取要求訊息（當 NPS 做為 RADIUS 伺服器使用時）。

- NPS 會將訊息轉送至另一個 RADIUS 伺服器（當 NPS 做為 RADIUS proxy 使用時）。

## <a name="configuring-the-nps-supplied-domain-name"></a>設定 NPS 提供的功能變數名稱

當使用者名稱不包含功能變數名稱時，NPS 會提供一個名稱。 根據預設，NPS 提供的功能變數名稱是 NPS 所屬的網域。 您可以透過下列登錄設定，指定 NPS 提供的功能變數名稱：

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>不當編輯登錄可能會對系統造成嚴重損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

某些非 Microsoft 網路存取伺服器會刪除或修改使用者所指定的功能變數名稱。 因此，網路存取要求會針對預設網域進行驗證，這可能不是使用者帳戶的網域。 若要解決此問題，請設定 RADIUS 伺服器，將使用者名稱變更為正確的功能變數名稱格式。
