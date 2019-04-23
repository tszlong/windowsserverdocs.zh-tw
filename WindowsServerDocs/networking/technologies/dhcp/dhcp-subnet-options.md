---
title: 子網路選取範圍的 DHCP 選項
description: 本主題提供 DHCP 子網路選取項目選項的動態主機設定通訊協定 (DHCP) Windows Server 2016 中的相關資訊。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.date: 08/17/2018
ms.openlocfilehash: 034ca48ef13a6bdac63ca99ac753fc9826460922
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870809"
---
# <a name="dhcp-subnet-selection-options"></a>子網路選取範圍的 DHCP 選項

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題的新 DHCP 子網路選取項目選項的相關資訊。

DHCP 現在支援選項 82\(子選項 5\)。 您可以使用這些選項，以允許 DHCP proxy 用戶端和轉接代理來要求特定的子網路，並從特定的 IP 位址範圍和範圍的 IP 位址。  如需詳細資訊，請參閱 <<c0>  **選項 82 子選項 5**:[RFC 3527 連結選取子選項 [轉接代理程式資訊] 選項的 DHCPv4](https://tools.ietf.org/html/rfc3527)。

如果您使用的設定為 DHCP 選項 82 DHCP 轉接代理，子選項 5，轉接代理程式可以要求來自特定 IP 位址範圍的 DHCP 用戶端的 IP 位址租用。


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>選項 82 子選項 5:連結的子選項

轉送代理程式的連結選擇子選項可讓 DHCP 轉接代理若要指定 IP 子網路的 DHCP 伺服器應該從中指派 IP 位址和選項。

通常，DHCP 轉接代理依賴閘道 IP 位址\(GIADDR\)欄位與 DHCP 伺服器進行通訊。 不過，GIADDR 受限於其兩個操作函式：

1. 若要通知的要求 IP 位址租用的 DHCP 用戶端所在的子網路中的 DHCP 伺服器。
2. 若要通知要用來與轉送代理程式通訊的 IP 位址的 DHCP 伺服器。

在某些情況下，用來與 DHCP 伺服器通訊的轉接代理程式的 IP 位址可能不同於從其 DHCP 用戶端 IP 位址，必須配置的 IP 位址範圍。 

選項 82 連結選取子選擇適合在此情況下，允許明確地陳述從中它想要的 IP 位址的子網路的配置將 DHCP v4 選項的形式在 82 子選項 5 轉送代理程式。

> [!NOTE]
>
> 所有的轉送代理程式 IP 位址 (GIADDR) 必須是作用中 DHCP 領域 IP 位址範圍的一部分。 DHCP 領域的 IP 位址範圍之外的任何 GIADDR 被視為惡意轉送且 Windows DHCP 伺服器也不會認可這些轉送代理程式從 DHCP 用戶端要求。
>
> 可以建立特殊的範圍，以 [授權] 轉接代理。 建立領域 GIADDR （或多個如果 GIADDR 是循序的 IP 位址）、 GIADDR 位址排除分佈，並接著啟動範圍。 這會授權轉接代理，同時防止 GIADDR 位址指派。


### <a name="use-case-scenario"></a>使用案例

在此案例中，組織網路包含 DHCP 伺服器和無線存取點\(AP\)來賓使用者。 從組織的 DHCP 伺服器-不過，因為防火牆原則限制指派來賓用戶端 IP 位址，DHCP 伺服器無法存取客體的無線網路或無線用戶端 broadcase 訊息。

若要解決這項限制，AP 設定來指定它想要從中客體中的用戶端，而同時指定會導致內部介面的 IP 位址 GIADDR 配置的 IP 位址的子網路連結選取子選項 5公司網路。
