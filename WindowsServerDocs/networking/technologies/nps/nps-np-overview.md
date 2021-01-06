---
title: 網路原則
description: 本主題概述 Windows Server 2016 中網路原則伺服器的網路原則，並提供 NPS 的其他指引連結。
manager: brianlic
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 8277b757ce5f4d8459ba39c9f29e8d47720b6e03
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946674"
---
# <a name="network-policies"></a>網路原則

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解 NPS 中的網路原則。

>[!NOTE]
>除了本主題之外，還提供下列網路原則檔。
> - [存取權限](nps-np-access.md)
> - [設定網路原則](nps-np-configure.md)

網路原則是條件、限制和設定的集合，可讓您指定誰可以獲得連線到網路的授權，以及在哪些情況下可以或不可以連線。

以遠端驗證撥入消費者服務 (RADIUS) 伺服器處理連線要求時，NPS 會對連線要求執行驗證和授權。 在驗證程序期間，NPS 會驗證連線到網路的使用者或電腦的識別身分。 在授權程序期間，NPS 會判定是否允許使用者或電腦存取網路。

為了進行這些決定，NPS 會使用 NPS 主控台中設定的網路原則。 NPS 也會檢查 Active Directory Domain Services AD DS 中使用者帳戶的撥入屬性 &reg; \( \) ，以執行授權。

## <a name="network-policies---an-ordered-set-of-rules"></a>網路原則-一組已排序的規則

網路原則可被視為規則。 每個規則都有一組條件及設定。 NPS 會比較規則的條件與連線要求的內容。 如果規則與連線要求相符，規則中所定義的設定就會套用到連線。

在 NPS 中設定多重網路原則時，它們是一組依序排列的規則。 NPS 會針對清單中第一個規則檢查每個連線要求，然後第二個，依此類推，直到找到相符的規則。

每個網路原則都有一個 **原則狀態** 設定，可讓您啟用或停用原則。 停用網路原則時，NPS 不會在授權連線要求時評估原則。

>[!NOTE]
>如果您希望 NPS 在執行連線要求的授權時評估網路原則，您必須選取 [已啟用原則] 核取方塊來設定 **原則狀態** 設定。

## <a name="network-policy-properties"></a>網路原則內容

每個網路原則都有四個類別的內容：

### <a name="overview"></a>總覽

 這些屬性可讓您指定是否啟用原則、原則是否授與或拒絕存取，以及連線要求是否需要特定的網路連線方法或網路存取伺服器的類型 (NAS) 。 概觀內容也可讓您指定是否忽略 AD DS 中使用者帳戶的撥入內容。 如果選取此選項，NPS 只用網路原則中的設定決定是否授權連線。


### <a name="conditions"></a>條件

 這些內容可讓您指定連線要求必須具備才能符合網路原則的條件；如果原則中設定的條件符合連線要求，NPS 會將網路原則中指定的設定套用到連線。 例如，如果您將 NAS IPv4 位址指定為網路原則的條件，且 NPS 收到來自具有指定 IP 位址之 NAS 的連線要求，則原則中的條件會符合連線要求。


### <a name="constraints"></a>條件約束

 限制是網路原則的額外參數，需要此參數才能符合連線要求。 如果限制與連線要求不符，NPS 會自動拒絕要求。 不同于 NPS 回應網路原則中不相符的狀況，如果不符合條件約束，NPS 會拒絕連線要求，而不會評估其他網路原則。

### <a name="settings"></a>設定

 如果原則的所有網路原則條件都相符，這些內容可讓您指定 NPS 套用到連線要求的設定。

當您使用 NPS 主控台新增網路原則時，必須使用 [新增網路原則嚮導]。 使用嚮導建立網路原則之後，您可以在 NPS 主控台中按兩下原則來自訂原則，以取得原則屬性。

如需模式比對語法指定網路原則屬性的範例，請參閱 [在 NPS 中使用正則運算式](nps-crp-reg-expressions.md)。

如需有關 NPS 的詳細資訊，請參閱 [網路原則伺服器 (nps) ](nps-top.md)。
