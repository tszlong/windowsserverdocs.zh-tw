---
title: 領域名稱
description: 本主題提供網路原則伺服器連接要求處理中的 Windows Server 2016 中使用領域名稱的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 29a835c9965c2115d0aac1ef9b704e05f430db8b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="realm-names"></a>領域名稱

>適用於：Windows Server（以每年次管道）、Windows Server 2016


您可以使用本主題的網路原則伺服器連接要求處理中使用領域名稱的概觀。

使用者名稱 RADIUS 屬性是字元字串，通常會包含使用者 account 位置和使用者 account 名稱。 使用者 account 位置也稱為領域或領域名稱，並為等於網域，包括 DNS 網域、Active Directory® 網域及 Windows nt4.0 網域的概念。 例如，如果位使用者的網域名稱 example.com 帳號資料庫中帳號，然後 example.com 是領域名稱。

在另一部範例中，如果您的使用者名稱 RADIUS 屬性包含的使用者名稱user1@example.com、user1 account 使用者名稱，example.com 且領域名稱。 開頭或結尾可以在 [使用者名稱呈現領域名稱：

- **Example\user1**。 在此範例中，領域名稱**範例**是前置詞。它也是 Active Directory 名稱&reg;Domain Services \(AD DS\) 網域。

- **user1@example.com**.在此範例中，領域名稱**example.com**是尾碼;而且它是 DNS 網域名稱或 AD DS 網域名稱。

您可以使用領域中時設計和部署 RADIUS 基礎結構連接要求原則設定的名稱，以確保連接要求的都會傳送從 RADIUS，也稱為網路存取伺服器的驗證並可以授權連接要求 RADIUS 伺服器。

NPS 設定為預設連接要求原則 RADIUS 伺服器、時 NPS 處理連接要求網域中的 NPS 伺服器是成員，以及受信任的網域。

若要設定做為 RADIUS proxy 和往後連接要求受信任的網域 NPS，您必須建立新連接要求原則。 在新連接要求原則中，您必須將會包含在您想要向前連接要求的使用者名稱屬性領域名稱與設定的使用者名稱屬性。 您還必須設定連接要求原則遠端 RADIUS 伺服器群組。 連接要求原則可 NPS 計算哪些連接要求轉寄給根據領域部分的使用者名稱屬性遠端 RADIUS 伺服器群組。

## <a name="acquiring-the-realm-name"></a>取得領域名稱

當使用者類型密碼認證期間連接嘗試或連接管理員（公分）設定檔使用者的電腦上已設定為自動提供領域名稱提供領域名稱部分的使用者名稱。

您可以指定時輸入認證網路連接嘗試在網路中的使用者，提供他們領域的名稱。

例如，您可以要求使用者輸入使用者名稱，包括 account 使用者名稱和領域名稱，在**的使用者名稱**在**連接**對話方塊中時建立連接撥號或 virtual 私人網路 (VPN)。

此外，如果您建立自訂撥號套件連接管理員管理組件 (CMAK) 使用時，您可以協助使用者透過公分設定檔使用者的電腦上已安裝的使用者名稱 account 自動新增領域名稱。 例如，您可以指定語法領域名稱和使用者名稱公分設定檔，讓使用者只有時輸入認證，指定的使用者 account 名稱。 在這個情況，使用者會不需要知道或記住的網域他們帳號所在的位置。

驗證程序期間使用者輸入的密碼型憑證後, 的使用者名稱被傳遞存取 client 從網路存取伺服器。 網路存取伺服器建構連接要求，並包含領域中的使用者名稱 RADIUS 屬性名稱傳送到或 RADIUS proxy 伺服器的存取要求訊息中。

如果 RADIUS 伺服器 NPS 伺服器，要求存取訊息被評估的一組設定的連接要求原則。 在連接要求原則條件可以包含使用者名稱屬性到的規格。

您可以設定連接要求原則的特定領域中收到的簡訊的使用者名稱屬性名稱是一組。 這可讓您建立路由規則 NPS RADIUS proxy 為時，向前 RADIUS 訊息特定領域組特定 RADIUS 伺服器的名稱。

## <a name="attribute-manipulation-rules"></a>屬性操作規則

RADIUS 訊息處理本機（當正 NPS RADIUS 伺服器為）或轉送到另一個 RADIUS 伺服器（使用 NPS RADIUS proxy 為）時之前，可以修改訊息中的使用者名稱屬性屬性操作規則。 您可以設定屬性操作規則的使用者名稱屬性，選取 [**的使用者名稱**在**條件**索引標籤中連接要求原則的屬性。 NPS 屬性操作規則使用標準運算式。

您可以設定屬性操作規則的使用者名稱屬性變更下列設定：

- 移除的使用者名稱領域名稱 \ (也稱為領域 stripping\)。 使用者名稱，例如user1@example.com以 user1 變更。

- 變更，但不是其語法領域名稱。 使用者名稱，例如user1@example.com以變更的user1@wcoast.example.com。

- 變更語法領域名稱。 若要變更使用者名稱 example\user1，例如user1@example.com。

根據您所設定的屬性操作規則的使用者名稱屬性修改之後，在第一次對應連接要求原則額外的設定用來判斷是否：

- NPS 伺服器處理存取要求訊息本機（時 NPS RADIUS 伺服器為正在使用）。

- NPS 伺服器轉送到另一個 RADIUS 伺服器的訊息（時 NPS RADIUS proxy 為正在使用）。

## <a name="configuring-the-the-nps-supplied-domain-name"></a>設定 NPS 提供網域名稱

當使用者名稱未包含網域名稱時、NPS 提供一個。 根據預設，NPS 提供網域名稱是網域之 NPS 伺服器。 您可以透過下列登錄設定 NPS 提供網域名稱：

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>編輯登錄錯誤可以嚴重損壞您的系統。 變更登錄以前，您應該備份在電腦上的任何重要的資料。

某些非 Microsoft 的網路存取伺服器 delete 或修改所指定的使用者的網域名稱。 因此，網路存取要求驗證預設網域，可能不是帳號的網域。 這個問題，設定您的使用者名稱變更成正確精確的網域名稱的格式的 RADIUS 伺服器。
