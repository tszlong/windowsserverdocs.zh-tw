---
title: 網路原則
description: 本主題提供的網路原則概觀針對 Windows Server 2016 中的網路原則伺服器，並包含 NPS 設計的額外指導的連結。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7e1604f4839bd955e5ea10d9eafea5ef0c978977
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-policies"></a>網路原則

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此主題中 NPS 的網路原則的概觀。

>[!NOTE]
>本主題中，除了下列網路原則文件會提供。
> - [存取權限](nps-np-access.md)
> - [設定原則的網路](nps-np-configure.md)

網路原則是設定的條件，限制和設定，可讓您指定獲得連上網路及下的人員或無法連接。

當處理連接要求為遠端驗證 Dial 使用者服務 (RADIUS) 伺服器、NPS 執行驗證與授權連接要求。 驗證程序期間 NPS 驗證身分的使用者或電腦已連接到網路。 授權程序期間 NPS 判斷是否允許的使用者或電腦來存取該網路。

為了讓這些判斷，NPS 使用已 NPS 主機的網路原則。 NPS 也會檢查的 Active Directory 中帳號撥號屬性&reg;Domain Services \(AD DS\) 執行的授權。

## <a name="network-policies---an-ordered-set-of-rules"></a>網路原則-排序組規則

網路原則可被視為規則。 每個規則具有條件和設定的設定。 NPS 比較連接要求的屬性規則條件。 如果相符項目，就會發生規則與連接要求，連接到套用定義規則中的設定。

多個網路原則設定 NPS 中，有一組排序的規則。 NPS 檢查清單中，然後在第二個，等等，第一個規則針對每個連接要求之前找出符合的。

每個網路原則有**原則狀態**設定，可讓您可以或停用的原則。 停用的網路原則，NPS 不會評估原則時授權連接要求。

>[!NOTE]
>如果您想要 NPS 評估的網路原則執行授權連接要求時，您必須設定**原則狀態**中選取原則設定支援核取方塊。

## <a name="network-policy-properties"></a>網路原則屬性

有四種針對每個的網路原則屬性：

### <a name="overview"></a>概觀

 這些屬性，可讓您指定是否已支援原則，是否原則授與拒絕存取，或特定網路連接方法或輸入網路存取伺服器 (NAS)、是否需要連接要求。 概觀屬性也可讓您指定是否忽略帳號 AD DS 在撥號屬性。 如果您選取此選項，在網路原則設定可 nps 判斷是否授權連接。


### <a name="conditions"></a>條件

 這些屬性，可讓您指定連接要求必須符合的網路原則; 以的條件如果在原則設定的條件符合連接要求，NPS 會套用指定在連接的網路原則設定。 例如，如果您的網路原則條件為指定 NAS IPv4 位址，NPS 從指定 IP 位址 NAS 接收連接要求原則中的條件符合連接要求。 


### <a name="constraints"></a>限制

 限制的其他符合連接要求所需的網路原則的參數。 連接要求不符合限制，如果 NPS 自動請求。 然而 NPS 回應不符合的條件中的網路原則，不符合限制，如果 NPS 拒絕連接要求而不需要評估額外的網路原則。

### <a name="settings"></a>設定

 這些屬性，可讓您指定 NPS 適用於連接要求如果符合所有的網路原則條件原則設定。

當您使用 NPS 主機新增新的網路原則時，您必須使用新的網路原則精靈。 您的網路原則建立使用精靈之後，您可以自訂原則，按兩下以取得原則的屬性 NPS 主控台原則。

範例模式比語法指定的網路原則屬性，請查看[使用規則運算式 NPS 在](nps-crp-reg-expressions.md)。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。
