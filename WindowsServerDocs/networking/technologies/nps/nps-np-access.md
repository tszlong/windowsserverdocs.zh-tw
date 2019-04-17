---
title: 存取權限
description: 本主題提供適用於 Windows Server 2016 中的網路原則伺服器的網路原則存取權限的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aeacfaeb159598d2e53bac29fb09ffc3e7739476
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="access-permission"></a>存取權限

>適用於：Windows Server（以每年次管道）、Windows Server 2016

上的存取權限已設定**概觀**索引標籤上的每個的網路原則的網路原則 Server (NPS)。 

這項設定可讓您設定的原則，授權或拒絕的存取權的使用者如果連接要求，以符合的條件和的網路原則約束。 

存取權限設定影響下列動作：

- **權限授與**。 存取會授與連接要求是否符合的條件與中原則設定的限制。
- **拒絕**。 存取如果連接要求符合的條件與中原則設定的限制。

存取權限授與或拒絕也根據您的設定的每個使用者 account 撥號屬性。

>[!NOTE]
>其中一個 Active Directory 使用者與電腦或 [本機使用者和群組 Microsoft Management Console \(MMC\) 嵌入式管理單元，視您是否有 Active Directory 設定帳號和屬性，撥號屬性，例如&reg;安裝 Domain Services (AD DS)。

使用者 account 設定**的網路存取權限**上的使用者帳號撥號屬性的設定、覆寫原則的網路存取權限設定。 當使用者 account 網路存取權限設定為**控制透過 NPS 的網路原則**選項，網路存取權限原則判斷使用者是否授與或無法存取。

>[!NOTE]
>在 Windows Server 2016 的預設值**的網路存取權限**在 AD DS 使用者 account 撥號屬性**控制 NPS 的網路原則透過**。

當 NPS 計算設定的網路原則對連接要求時，它會執行下列動作：

- 如果不符合的條件的第一個原則，NPS 評估的下一步原則，並持續此程序，直到或找出符合的所有原則都評估相符項目。
- 符合的條件與原則的限制，如果 NPS 授與或拒絕存取權，根據原則中的存取權限設定的值。
- 如果不符合的條件的原則相符項目，但原則中的限制，NPS 請求連接。
- 如果不符合的條件的所有原則、NPS 請求連接。

## <a name="ignore-user-account-dial-in-properties"></a>忽略撥號屬性 account 使用者

您可以設定 NPS 的網路原則來選取或清除 [略過的帳號撥號屬性**忽略使用者 account 撥號屬性**核取方塊，在**概觀**索引標籤的網路原則。 

通常時 NPS 執行連接要求的授權，它就會檢查使用者帳號，並將值設定的網路存取權限，可能會影響是否使用者授權連上網路的撥號屬性。 當您設定的帳號撥號屬性忽略期間授權 NPS 時，網路原則設定判斷使用者是否會授與網路的存取權。

撥號的屬性帳號包含下列動作：

- 網路存取權限
- 本機號碼
- 回呼選項
- 靜態 IP 位址
- 靜態路徑

若要支援多種連接 NPS 提供驗證與授權，可能必須停用的使用者 account 撥號屬性處理。 這可以的案例中，特定撥號屬性就不需要的支援。

範例、本機號碼、回呼、靜態 IP 位址，和屬性的設計網路的存取伺服器撥打 client 靜態路由 \(NAS\)，不是為可連接到 wireless 存取點。 Wireless 存取點 NPS RADIUS 郵件中收到這些設定，可能無法處理，這可能會造成 wireless client 會中斷。

當 NPS 提供驗證和撥號中的，您組織的網路存取透過 wireless 存取點的使用者的授權時，您必須設定撥號屬性，以支援任一撥號連接 \（來設定撥號 properties\) 或 wireless 連接 \（由不設定撥號 properties\）。

您可以撥號屬性處理有時候帳號，以便使用 NPS \ (例如撥號-in\)，並在撥號屬性處理其他案例中停用 \（例如 802.1 X wireless 與驗證 switch\)。

您也可以使用**忽略使用者 account 撥號屬性**來管理網路存取控制透過群組和原則的網路上的存取權限設定。 當您選取 [**忽略使用者 account 撥號屬性**核取方塊，帳號網路存取權限會略過。

這個設定只缺點是，您無法使用的其他使用者 account 撥號屬性本機號碼、回呼、靜態 IP 位址，以及靜態路徑。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。
