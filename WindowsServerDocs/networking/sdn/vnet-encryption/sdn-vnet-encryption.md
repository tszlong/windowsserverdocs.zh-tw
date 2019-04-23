---
title: 虛擬網路加密
description: 虛擬網路加密可加密標示為 '啟用加密。' 的子網路內彼此通訊的虛擬機器之間的虛擬網路流量
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: f2f50ae3146854e2ef6081b0c400a474b53dcf66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851079"
---
# <a name="virtual-network-encryption"></a>虛擬網路加密

>適用於：Windows Server

虛擬網路加密可加密標示為 '啟用加密。' 的子網路內彼此通訊的虛擬機器之間的虛擬網路流量 這項功能也利用虛擬子網路上的資料包傳輸層安全性 (DTLS) 來加密封包。 DTLS 提供保護以防止任何可存取實體網路的人進行竊聽、竄改和偽造。

虛擬網路的加密需要：
- 安裝在每個已啟用 SDN 的 HYPER-V 主機上的加密憑證。
- 參考憑證的指紋網路控制卡中的認證物件。
- 每個虛擬網路上的組態包含需要加密的子網路。

一旦您啟用的子網路上的加密，該子網路內的所有網路流量會自動都加密除了可能也需要進行任何應用程式層級加密。  自動即使標示為已加密，跨越子網路的流量會傳送未加密。 跨越網路界限的任何流量也取得傳送未加密。

>[!TIP]
>如果您必須限制只有已加密的子網路上通訊的應用程式，您可以使用存取控制清單 (Acl) 只是為了讓目前的子網路內的通訊。 如需詳細資訊，請參閱 <<c0> [ 使用存取控制清單 (Acl) 來管理資料中心網路流量流動](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)。

### <a name="next-steps"></a>後續步驟

[設定虛擬網路的加密](https://docs.microsoft.com/windows-server/networking/sdn/vnet-encryption/sdn-config-vnet-encryption)

