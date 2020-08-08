---
title: 虛擬網路加密
description: 虛擬網路加密可讓虛擬機器之間的虛擬網路流量進行加密，這兩者會在子網內彼此通訊，並標示為「啟用加密」。
manager: grcusanz
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: 68014e0941205db61cc0b607e6784fb8d6d807ab
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955865"
---
# <a name="virtual-network-encryption"></a>虛擬網路加密

>適用于： Windows Server

虛擬網路加密可讓虛擬機器之間的虛擬網路流量進行加密，這兩者會在子網內彼此通訊，並標示為「啟用加密」。 這項功能也利用虛擬子網路上的資料包傳輸層安全性 (DTLS) 來加密封包。 DTLS 提供保護以防止任何可存取實體網路的人進行竊聽、竄改和偽造。

虛擬網路加密需要：
- 已安裝在每個 SDN 的 Hyper-v 主機上的加密憑證。
- 網路控制卡中的認證物件，其參考該憑證的指紋。
- 每個虛擬網路上的設定都包含需要加密的子網。

一旦您在子網上啟用加密，該子網內的所有網路流量都會自動加密，以及任何可能也會發生的應用層級加密。  跨越子網的流量即使標示為已加密，也會自動以未加密的方式傳送。 任何跨越虛擬網路界限的流量也會以未加密的形式傳送。

>[!TIP]
>如果您必須將應用程式限制為只能在加密的子網上進行通訊，您可以使用存取控制清單 (Acl) 只允許目前子網內的通訊。 如需詳細資訊，請參閱[使用存取控制清單 (acl) 來管理資料中心網路流量](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)。

### <a name="next-steps"></a>後續步驟

[設定虛擬網路的加密](https://docs.microsoft.com/windows-server/networking/sdn/vnet-encryption/sdn-config-vnet-encryption)

