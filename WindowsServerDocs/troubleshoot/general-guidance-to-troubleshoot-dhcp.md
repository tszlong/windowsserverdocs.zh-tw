---
title: 疑難排解 DHCP 的一般指引
description: 本 artilce 引進疑難排解 DHCP 的一般指引。
ms.prod: windows-server
ms.service: na
manager: dcscontentpm
ms.technology: server-general
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: c0460791fef2451722af09e8bbe08b51a605f01b
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150156"
---
# <a name="general-guidance-to-troubleshoot-dhcp"></a>疑難排解 DHCP 的一般指引

開始進行疑難排解之前，請先檢查下列專案。 這些可協助您找出問題的根本原因。

## <a name="checklist"></a>檢查清單

  - 問題何時開始發生?

  - 是否有任何錯誤訊息？

  - DHCP 伺服器先前是否正常運作，或從未執行過？  
    如果先前已正常運作，則在問題開始之前是否有任何變更。 例如，是否已安裝更新？ 對基礎結構所做的變更嗎？

  - 問題是持續性還是間歇性的？ 如果它是間歇性的，那麼最後發生的時間為何？

  - 所有用戶端或只有特定用戶端（例如單一範圍子網）都會發生位址租用失敗？

  - 與 DHCP 伺服器在相同的網路子網上是否有任何用戶端？

  - 如果用戶端位於相同的網路子網上，可以取得 IP 位址嗎？

  - 如果用戶端不在相同的網路子網上，路由器或 VLAN 交換器是否正確設定為具有 DHCP 轉送代理程式（也稱為 IP 協助程式）？

  - DHCP 伺服器是獨立的或是設定為高可用性，例如分割範圍或 DHCP 容錯移轉？

  - 檢查中繼裝置的功能，例如 VRRP/HSRP、動態 ARP 檢查，或已知導致問題的 DHCP 窺探。
