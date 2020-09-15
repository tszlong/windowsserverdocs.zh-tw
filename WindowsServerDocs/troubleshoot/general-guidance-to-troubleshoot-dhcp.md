---
title: 針對 DHCP 進行疑難排解的一般指導方針
description: 本 artilce 介紹疑難排解 DHCP 的一般指引。
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: e5550654beb0f303be946358c171f3a1b197ea40
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078645"
---
# <a name="general-guidance-to-troubleshoot-dhcp"></a>針對 DHCP 進行疑難排解的一般指導方針

在開始進行疑難排解之前，請先檢查下列專案。 這些可協助您找出問題的根本原因。

## <a name="checklist"></a>檢查清單

  - 問題何時開始發生？

  - 是否有任何錯誤訊息？

  - DHCP 伺服器先前是否正常運作，或從未運作過？
    如果之前已有任何變更，則會在問題開始之前進行任何變更。 例如，是否已安裝更新？ 對基礎結構進行了變更嗎？

  - 問題是持續或間歇性的？ 如果它是間歇性的，上次發生的時間為何？

  - 所有用戶端或只針對特定用戶端（例如單一範圍子網）都會發生位址租用失敗嗎？

  - 與 DHCP 伺服器相同的網路子網上是否有任何用戶端？

  - 如果用戶端位於相同的網路子網上，是否可以取得 IP 位址？

  - 如果用戶端不在相同的網路子網上，路由器或 VLAN 交換器是否已正確設定為具有 DHCP 轉送代理程式 (也稱為 IP 協助程式) ？

  - DHCP 伺服器是獨立的，還是設定為高可用性，例如分割範圍或 DHCP 容錯移轉？

  - 檢查中繼裝置的功能，例如 VRRP/HSRP、動態 ARP 檢查，或是已知造成問題的 DHCP 窺探。
