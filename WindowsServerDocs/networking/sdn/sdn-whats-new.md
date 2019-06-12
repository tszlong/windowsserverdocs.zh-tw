---
title: 在適用於 Windows Server 的 SDN 中最新消息
description: 此主題提供有關新的軟體定義網路功能的 Windows Server 1709
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: aef2bc32f249550d4e8d33d4b871ca98010e2f49
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446295"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>適用於 Windows Server 2019 的 SDN 的新功能

>適用於：Windows Server (半年度管道)


|                         **功能**                          |                                                                                                                                                                                         **描述**                                                                                                                                                                                         | **新增/更新** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [加密的網路](vnet-encryption/sdn-vnet-encryption.md) | 虛擬網路加密可加密標示為 '啟用加密。' 的子網路內彼此通訊的虛擬機器之間的虛擬網路流量 這項功能也利用虛擬子網路上的資料包傳輸層安全性 (DTLS) 來加密封包。 DTLS 提供保護以防止任何可存取實體網路的人進行竊聽、竄改和偽造。 |       新的       |
|    [防火牆稽核](security/sdn-firewall-auditing.md)    |                                                                                            防火牆稽核是一項新功能在 Windows Server 2019 SDN 防火牆。 當您啟用 SDN 防火牆時，取得記錄中任何具有已啟用記錄的 SDN 防火牆規則 (Acl) 所處理的流程。                                                                                            |       新的       |
| [虛擬網路對等互連](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      虛擬網路對等互連可讓您順暢地連線兩個虛擬網路。 一旦對等互連，進行連線，虛擬網路會顯示為其中。                                                                                                                      |       新的       |
|           [輸出計量](manage/sdn-egress.md)            |                  在 Windows Server 2019 這項新功能可讓 SDN 提供使用量計量的輸出資料傳輸。 加入這項功能，與網路控制站會保留每個虛擬網路的所有 IP 範圍列入允許清單用於 SDN，並考慮任何繫結不會納入為這些範圍的其中一個目的地的封包會計費的輸出資料傳輸。                   |       新的       |

---



