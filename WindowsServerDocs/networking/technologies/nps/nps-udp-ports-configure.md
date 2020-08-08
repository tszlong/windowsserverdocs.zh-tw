---
title: 設定 NPS UDP 連接埠資訊
description: 您可以使用本主題設定網路原則伺服器 (NPS) 用於遠端驗證撥入使用者服務的埠， (RADIUS) Windows Server 2016 中的驗證和帳戶處理流量。
manager: brianlic
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c522206fe701d47f8f0c07e7c7a64d7e6fc7074c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944508"
---
# <a name="configure-nps-udp-port-information"></a>設定 NPS UDP 連接埠資訊

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用下列程式，設定網路原則伺服器 (NPS) 用來進行遠端驗證撥入使用者服務 \( RADIUS \) 驗證和帳戶處理流量的埠。

根據預設值，NPS 會接聽埠1812、1813、1645和1646的 RADIUS 流量，以取得適用于 \( 所有已安裝網路介面卡的網際網路通訊協定第6版 IPv6 \) 和 IPv4。

>[!NOTE]
>如果您解除安裝網路介面卡上的 IPv4 或 IPv6，NPS 不會監視已解除安裝的通訊協定的 RADIUS 流量。

用於驗證的通訊埠值1812和用於計量的1813，是由網際網路工程工作 \( \) 在 rfc 2865 和2866中強制執行 IETF 所定義的 RADIUS 標準埠。 不過，根據預設，許多存取伺服器會針對驗證要求使用埠1645，並針對帳戶處理要求使用1646。 無論您決定使用哪些連接埠號碼，只要確定 NPS 與存取伺服器是設定成使用相同的連接埠號碼。

>重大如果您未使用 RADIUS 預設通訊埠編號，您必須在防火牆上針對本機電腦設定例外狀況，以允許新埠上的 RADIUS 流量。 如需詳細資訊，請參閱[設定 RADIUS 流量的防火牆](nps-firewalls-configure.md)。

至少要有 **Domain Admins** 的成員資格或是對等成員資格，才能完成此程序。

## <a name="to-configure-nps-udp-port-information"></a>設定 NPS UDP 埠資訊

1. 開啟 NPS 主控台。
2. 以滑鼠右鍵按一下 [**網路原則伺服器**]，**然後按一下 [** 內容]。
3. 按一下 [**埠**] 索引標籤，然後檢查 [埠] 的設定。 如果您的 RADIUS 驗證和 RADIUS 帳戶處理 UDP 埠因驗證所提供的預設值 (1812 和1645，以及用於計量的1813和 1646) ，請在 [**驗證**和**帳戶**處理] 中輸入您的埠設定。
4. 驗證或帳戶處理要求若要使用多個連接埠設定，請以逗號分隔連接埠號碼。

如需管理 NPS 的詳細資訊，請參閱[管理網路原則伺服器](nps-manage-top.md)。

如需有關 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS) ](nps-top.md)。
