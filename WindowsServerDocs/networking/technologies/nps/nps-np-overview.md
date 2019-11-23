---
title: 網路原則
description: 本主題概要說明 Windows Server 2016 中網路原則伺服器的網路原則，並包含 NPS 其他指引的連結。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c241191f54080ed92a1f1a274c0b2aff0b8c564c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405350"
---
# <a name="network-policies"></a>網路原則

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題，以取得 NPS 中的網路原則總覽。

>[!NOTE]
>除了本主題之外，也提供下列網路原則檔。
> - [存取權限](nps-np-access.md)
> - [設定網路原則](nps-np-configure.md)

網路原則是條件、限制和設定的集合，可讓您指定誰已獲授權連線到網路，以及他們可以或無法連線的情況。

以遠端驗證撥入使用者服務（RADIUS）伺服器的形式處理連線要求時，NPS 會對連線要求執行驗證和授權。 在驗證過程中，NPS 會驗證連線到網路的使用者或電腦的身分識別。 在授權程式期間，NPS 會決定是否允許使用者或電腦存取網路。

為了做出這些決定，NPS 會使用 NPS 主控台中設定的網路原則。 NPS 也會在 Active Directory&reg; 網域服務 \(AD DS\) 中，檢查使用者帳戶的撥入內容，以執行授權。

## <a name="network-policies---an-ordered-set-of-rules"></a>網路原則-一組已排序的規則

網路原則可以視為規則。 每個規則都有一組條件和設定。 NPS 會比較規則的條件與連線要求的屬性。 如果規則與連線要求之間發生比對，則規則中定義的設定會套用到連接。

在 NPS 中設定多個網路原則時，它們是一組已排序的規則。 NPS 會針對清單中的第一個規則檢查每個連線要求，然後針對第二個，依此類推，直到找到相符的。

每個網路原則都有一個**原則狀態**設定，可讓您啟用或停用原則。 當您停用網路原則時，NPS 不會在授權連接要求時評估原則。

>[!NOTE]
>如果您想要 NPS 在執行連線要求的授權時評估網路原則，您必須選取 [已啟用原則] 核取方塊來設定**原則狀態**設定。

## <a name="network-policy-properties"></a>網路原則內容

每個網路原則都有四個類別的屬性：

### <a name="overview"></a>概觀

 這些屬性可讓您指定是否要啟用原則、原則是否會授與或拒絕存取權，以及連線要求是否需要特定的網路連線方法或網路存取伺服器（NAS）類型。 總覽屬性也可讓您指定是否忽略 AD DS 中使用者帳戶的撥入屬性。 如果您選取此選項，NPS 只會使用網路原則中的設定來判斷連線是否已獲授權。


### <a name="conditions"></a>條件

 這些屬性可讓您指定連線要求必須擁有才能符合網路原則的條件;如果原則中所設定的條件符合連線要求，NPS 會將網路原則中指定的設定套用至連線。 例如，如果您將 NAS IPv4 位址指定為網路原則的條件，而 NPS 收到來自具有指定 IP 位址之 NAS 的連線要求，原則中的條件就會符合連線要求。 


### <a name="constraints"></a>限制式

 條件約束是網路原則的其他參數，必須符合連接要求。 如果條件約束不符合連線要求，NPS 會自動拒絕要求。 不同于 NPS 回應網路原則中的不相符條件，如果條件約束不相符，NPS 會拒絕連線要求，而不會評估額外的網路原則。

### <a name="settings"></a>設定

 如果原則的所有網路原則條件都相符，這些屬性可讓您指定 NPS 套用至連線要求的設定。

當您使用 NPS 主控台新增網路原則時，必須使用 [新增網路原則] Wizard。 使用嚮導建立網路原則之後，您可以在 NPS 主控台中按兩下原則來自訂原則，以取得原則屬性。

如需模式比對語法指定網路原則屬性的範例，請參閱[在 NPS 中使用正則運算式](nps-crp-reg-expressions.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。
