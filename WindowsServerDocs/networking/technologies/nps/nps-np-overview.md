---
title: 網路原則
description: 本主題提供 Windows Server 2016 中的網路原則伺服器的網路原則的概觀，並包含 NPS 的其他指導連結。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ab80bb6cf26578430b76806405a65a0f596ef2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848949"
---
# <a name="network-policies"></a>網路原則

>適用於：Windows Server （半年通道），Windows Server 2016

如需在 NPS 網路原則的概觀，您可以使用本主題。

>[!NOTE]
>本主題中，除了下列的網路原則文件使用。
> - [存取權限](nps-np-access.md)
> - [設定網路原則](nps-np-configure.md)

網路原則是條件、 限制和設定，可讓您指定誰獲得授權連線到網路，以及哪些情況下它們可以或無法連線的集合。

當為遠端驗證撥號使用者服務 (RADIUS) 伺服器處理連線要求，NPS，則會執行驗證和授權連線要求。 在驗證過程中，NPS 會驗證連線到網路的電腦或使用者的身分識別。 授權期間，NPS 會判斷是否允許使用者或電腦存取網路。

若要讓這些決定，NPS 會使用 NPS 主控台中所設定的網路原則。 NPS 也會檢查 Active Directory 中的使用者帳戶撥入內容&reg;網域服務\(AD DS\)執行授權。

## <a name="network-policies---an-ordered-set-of-rules"></a>網路原則-一組排序的規則

網路原則可視為規則。 每個規則有一組條件和設定。 NPS 會比較與連線要求的屬性規則的條件。 如果規則與連線要求相符，規則中定義的設定會套用至連線。

當在 NPS 中設定多個網路原則時，它們會是一組排序的規則。 NPS 檢查第一個規則，在清單中，則第二個，依此類推，針對每個連線要求，直到找到相符項目。

每個網路原則都**原則狀態**設定，可讓您啟用或停用原則。 當您停用網路原則時，NPS 不會評估原則授權連線要求時。

>[!NOTE]
>如果您想要 NPS 執行連線要求授權時，評估網路原則，您必須設定**原則狀態**藉由選取 原則 設定已啟用 核取方塊。

## <a name="network-policy-properties"></a>網路原則內容

有四個類別的每個網路原則屬性：

### <a name="overview"></a>總覽

 這些屬性可讓您指定是否啟用原則、 原則是否會授與或拒絕存取，以及特定的網路連線方法或網路存取伺服器 (NAS) 的型別是否需要針對連接要求。 概觀內容也可讓您指定是否忽略 AD DS 中的使用者帳戶撥入內容。 如果您選取此選項時，只將網路原則中會使用設定 NPS 來判斷是否授權連線。


### <a name="conditions"></a>條件

 這些屬性可讓您指定連線要求必須具備才能符合網路原則的條件如果原則中設定的條件符合連線要求，NPS 會套用至連線的網路原則中指定的設定。 比方說，如果您指定 NAS IPv4 位址的網路原則條件，NPS 從擁有指定的 IP 位址的 NAS 收到連線要求原則中的條件符合連線要求。 


### <a name="constraints"></a>限制式

 條件約束是網路原則尋找相符的連線要求所需的其他參數。 如果連線要求不符合條件約束時，NPS 會自動拒絕要求。 不同於 NPS 回應網路原則中有不相符的條件，如果不符合條件約束時，NPS 會拒絕連線要求而不需要評估其他網路原則。

### <a name="settings"></a>設定

 這些屬性可讓您指定的設定，NPS 會套用至連線的要求，如果所有原則的網路原則條件的比對。

當您新增新的網路原則使用 NPS 主控台時，您必須使用新的網路原則精靈。 使用精靈建立網路原則之後，您可以自訂原則，按兩下 NPS 主控台，以取得原則內容中的原則。

如範例的模式比對語法指定網路原則屬性，請參閱 < [NPS 中的使用規則運算式](nps-crp-reg-expressions.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。
