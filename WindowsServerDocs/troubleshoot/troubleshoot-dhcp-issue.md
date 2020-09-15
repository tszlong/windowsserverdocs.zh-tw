---
title: 動態主機設定通訊協定 (DHCP) 的疑難排解指南
description: 本 artilce 介紹 DHCP 問題的疑難排解指南。
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 92a7984fe2070ac194aa01a5a9aa63e85b7aa45d
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078615"
---
# <a name="troubleshooting-guide-for-dynamic-host-configuration-protocol-dhcp"></a>動態主機設定通訊協定 (DHCP) 的疑難排解指南

針對任何裝置 (例如電腦或電話) 能夠在網路中操作，必須將 IP 位址指派給它。 您可以手動或自動指派 IP 位址。 自動指派是由 (Microsoft 或協力廠商伺服器) 的 DHCP 服務所處理。

在本文中，我們將討論 Microsoft IPv4 DHCP 用戶端和伺服器的一般疑難排解步驟。

## <a name="more-information"></a>更多資訊

IPv4 位址指派的程式通常牽涉到三個主要元件：

- 必須取得 IP 位址的 DHCP 用戶端裝置

- 根據特定設定將 IP 位址提供給用戶端的 DHCP 服務

- DHCP 轉接代理/IP 協助程式，可將 DHCP 廣播要求傳送到不同的網路區段

DHCP 用戶端對伺服器的通訊是由兩個對等之間的三種互動所組成：

- 以**廣播為基礎的 DORA** (探索、供應專案、要求、確認) 。 此程序由下列步驟組成：

    - DHCP 用戶端會將 DHCP 探索廣播要求傳送給範圍內所有可用的 DHCP 伺服器。

    - 從 DHCP 伺服器接收 DHCP 供應專案廣播回應，提供可用的 IP 位址租用。

    - DHCP 用戶端廣播要求會要求所提供的 IP 位址租用和 DHCP 廣播通知結尾。

    - 如果 DHCP 用戶端和伺服器位於不同的邏輯網路區段，DHCP 轉送代理程式會作用轉寄站，並在對等之間來回傳送 DHCP 廣播封包。

- **單播 DHCP 更新要求**：這些要求會從 dhcp 用戶端直接傳送到 dhcp 伺服器，以在 ip 位址租用時間的50% 後更新 ip 位址指派。

- 重新系結**DHCP 廣播要求**：這些要求會在用戶端範圍內的任何 DHCP 伺服器上進行。 這些會在 IP 位址租用期間的87.5% 之後傳送，因為這表示導向的單播要求沒有作用。 針對 DORA 程式，此程式牽涉到 DHCP 轉送代理程式通訊。

如果 Microsoft DHCP 用戶端未收到有效的 DHCP IPv4 位址，則可能會將用戶端設定為使用 APIPA 位址。 如需詳細資訊，請參閱下列知識庫文章： [220874](https://support.microsoft.com/help/220874) 如何在不使用 DHCP 伺服器的情況下使用自動 tcp/ip 定址

所有通訊都是在 UDP 埠67和68上完成。 如需詳細資訊，請參閱下列知識庫文章： [169289](https://support.microsoft.com/help/169289) DHCP (動態主機設定通訊協定) 基本概念。