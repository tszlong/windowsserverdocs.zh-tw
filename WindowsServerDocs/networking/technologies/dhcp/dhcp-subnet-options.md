---
title: DHCP 子網選擇選項
description: 本主題提供有關 Windows Server 2016 中的動態主機設定通訊協定 (DHCP) 之 DHCP 子網選擇選項的資訊。
manager: dougkim
ms.topic: how-to
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: lizross
author: eross-msft
ms.date: 08/17/2018
ms.openlocfilehash: 076ca8c41616b77cb7503ca960667771c8f6f412
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948794"
---
# <a name="dhcp-subnet-selection-options"></a>DHCP 子網選擇選項

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，以取得有關新的 DHCP 子網選擇選項的資訊。

DHCP 現在支援選項 82 \( 子選項 5 \) 。 您可以使用這些選項，讓 DHCP proxy 用戶端和轉送代理程式要求特定子網的 IP 位址，以及從特定的 IP 位址範圍和範圍要求 IP 位址。  如需詳細資訊，請參閱適用于 [DHCPv4 的轉送代理程式資訊選項](https://tools.ietf.org/html/rfc3527)的 **選項82子選項 5**： RFC 3527 連結選取子選項。

如果您使用以 DHCP 選項82（子選項5）設定的 DHCP 轉接代理，則轉送代理程式可以向特定 IP 位址範圍要求 DHCP 用戶端的 IP 位址租用。


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>選項 82 Sub Option 5： Link Selection Sub 選項

[轉送代理連結選擇] 子選項可讓 DHCP 轉接代理指定 DHCP 伺服器應指派 IP 位址和選項的 IP 子網。

通常，DHCP 轉送代理程式依賴閘道 IP 位址 \( GIADDR \) 欄位來與 DHCP 伺服器通訊。 不過，GIADDR 受限於它的兩個操作功能：

1. 通知 DHCP 伺服器有關要求 IP 位址租用的 DHCP 用戶端所在的子網。
2. 通知 DHCP 伺服器使用 IP 位址來與轉送代理程式通訊。

在某些情況下，轉送代理程式用來與 DHCP 伺服器通訊的 IP 位址，可能會與需要從中配置 DHCP 用戶端 IP 位址的 IP 位址範圍不同。

在這種情況下，選項82的連結選擇子選項很有用，可讓轉接代理明確地陳述它想要以 DHCP v4 選項82子選項5形式配置之 IP 位址的子網。

> [!NOTE]
>
> 所有轉送代理程式 IP 位址 (GIADDR) 都必須是作用中 DHCP 範圍 IP 位址範圍的一部分。 DHCP 領域 IP 位址範圍以外的任何 GIADDR 都會被視為 rogue 轉送，且 Windows DHCP 伺服器不會向這些轉送代理程式確認 DHCP 用戶端要求。
>
> 您可以建立特殊範圍來「授權」轉送代理程式。 建立具有 GIADDR (的範圍，如果 GIADDR 是) 的連續 IP 位址，請將 GIADDR 位址 (es) 從散發中排除，然後啟動範圍。 這會授權轉送代理程式，同時防止指派 GIADDR 位址。


### <a name="use-case-scenario"></a>使用案例

在此案例中，組織網路包含來賓使用者的 DHCP 伺服器和無線存取點 \( AP \) 。 來賓用戶端 IP 位址會從組織 DHCP 伺服器指派-不過，由於防火牆原則的限制，DHCP 伺服器無法存取具有 broadcase 訊息的來賓無線網路或無線用戶端。

若要解決這項限制，您可以使用連結選擇子選項5來設定 AP，以指定要為來賓用戶端配置 IP 位址的子網，同時在 GIADDR 中指定導致公司網路的內部介面 IP 位址。
