---
title: 虛擬網路加密
description: 虛擬網路加密允許加密虛擬機器之間的虛擬網路流量，這些虛擬機器會在標示為「啟用加密」的子網內彼此通訊。
manager: grcusanz
ms.topic: how-to
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: ef2d3f1adf69d3dedad4e77e6afca9b61eec4616
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97945874"
---
# <a name="virtual-network-encryption"></a>虛擬網路加密

>適用于： Windows Server

虛擬網路加密允許加密虛擬機器之間的虛擬網路流量，這些虛擬機器會在標示為「啟用加密」的子網內彼此通訊。 這項功能也利用虛擬子網路上的資料包傳輸層安全性 (DTLS) 來加密封包。 DTLS 提供保護以防止任何可存取實體網路的人進行竊聽、竄改和偽造。

虛擬網路加密需要：
- 在每個啟用 SDN 的 Hyper-v 主機上安裝的加密憑證。
- 網路控制卡中的認證物件，其參考該憑證的指紋。
- 每個虛擬網路上的設定都包含需要加密的子網。

一旦在子網上啟用加密，該子網內的所有網路流量都會自動加密，除了可能也會發生的任何應用層級加密之外。  子網之間的流量如果標示為已加密，則會自動以未加密的方式傳送。 任何跨越虛擬網路界限的流量也會以未加密的形式傳送。

>[!TIP]
>如果您必須將應用程式限制為只在加密子網上進行通訊，您可以使用 (Acl) 的存取控制清單，以允許目前子網內的通訊。 如需詳細資訊，請參閱 [使用 (acl 的存取控制清單) 管理資料中心網路流量流程](../manage/use-acls-for-traffic-flow.md)。

### <a name="next-steps"></a>後續步驟

[設定虛擬網路的加密](./sdn-config-vnet-encryption.md)