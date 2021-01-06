---
title: 設定 NPS UDP 連接埠資訊
description: 您可以使用本主題來設定網路原則伺服器 (NPS) 用來遠端驗證撥入消費者服務 (RADIUS 的埠，) 驗證和 Windows Server 2016 中的帳戶處理流量。
manager: brianlic
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 94a8e67056dff497f1adc18d898a6c87f4ab665c
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947434"
---
# <a name="configure-nps-udp-port-information"></a>設定 NPS UDP 連接埠資訊

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用下列程式來設定網路原則伺服器 (NPS) 用於遠端驗證撥入消費者服務 \( RADIUS \) 驗證和帳戶處理流量的埠。

根據預設，NPS 會在埠1812、1813、1645和1646上接聽適用于 \( \) 所有已安裝網路介面卡的網際網路通訊協定第6版 IPv6 和 IPV4 的 RADIUS 流量。

>[!NOTE]
>如果您解除安裝網路介面卡上的 IPv4 或 IPv6，NPS 不會監視已解除安裝的通訊協定的 RADIUS 流量。

用於進行驗證的埠值1812和用於計量的1813，是由網際網路工程任務推動小組 \( \) 在 rfc 2865 和2866中定義的 RADIUS 標準埠。 不過，根據預設，許多存取伺服器會針對驗證要求使用埠1645，並針對帳戶要求使用1646。 無論您決定使用哪些連接埠號碼，只要確定 NPS 與存取伺服器是設定成使用相同的連接埠號碼。

>須知如果您未使用 RADIUS 預設通訊埠號碼，您必須在防火牆上設定本機電腦的例外狀況，以允許新埠上的 RADIUS 流量。 如需詳細資訊，請參閱 [設定 RADIUS 流量的防火牆](nps-firewalls-configure.md)。

至少要有 **Domain Admins** 的成員資格或是對等成員資格，才能完成此程序。

## <a name="to-configure-nps-udp-port-information"></a>設定 NPS UDP 埠資訊

1. 開啟 NPS 主控台。
2. 在 [ **網路原則伺服器**] 上按一下滑鼠右鍵， **然後按一下 [** 內容]。
3. 按一下 [ **埠** ] 索引標籤，然後檢查埠的設定。 如果您的 RADIUS 驗證和 RADIUS 帳戶處理 UDP 埠與提供的預設值不同 (1812 和1645以進行驗證，而1813和1646用於帳戶處理) ，請在 [ **驗證** 與 **帳戶** 處理] 中輸入您的埠設定。
4. 驗證或帳戶處理要求若要使用多個連接埠設定，請以逗號分隔連接埠號碼。

如需有關管理 NPS 的詳細資訊，請參閱 [管理網路原則伺服器](nps-manage-top.md)。

如需有關 NPS 的詳細資訊，請參閱 [網路原則伺服器 (nps) ](nps-top.md)。
