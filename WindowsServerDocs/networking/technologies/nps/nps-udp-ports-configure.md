---
title: 設定 NPS UDP 連接埠資訊
description: 您可以使用本主題來設定網路原則伺服器（NPS）用來進行遠端驗證撥入使用者服務（RADIUS）驗證的通訊埠，以及 Windows Server 2016 中的帳戶處理流量。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f874469666ab9b68d9eb970cf7fcb6a89ef27f0c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315575"
---
# <a name="configure-nps-udp-port-information"></a>設定 NPS UDP 連接埠資訊

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用下列程式，設定網路原則伺服器（NPS）用來進行遠端驗證撥入使用者服務的埠，\(RADIUS\) 驗證和帳戶處理流量。

根據預設值，NPS 會接聽埠1812、1813、1645和1646上的 RADIUS 流量，以用於網際網路通訊協定第6版 \(IPv6\) 和 IPv4 用於所有已安裝的網路介面卡。

>[!NOTE]
>如果您解除安裝網路介面卡上的 IPv4 或 IPv6，NPS 不會監視已解除安裝的通訊協定的 RADIUS 流量。

用於驗證的1812埠值和1813的計量是網際網路工程任務推動小組所定義的 RADIUS 標準埠，\(Rfc 2865 和2866中的 IETF\)。 不過，根據預設，許多存取伺服器會針對驗證要求使用埠1645，並針對帳戶處理要求使用1646。 無論您決定使用哪些連接埠號碼，只要確定 NPS 與存取伺服器是設定成使用相同的連接埠號碼。

>重大如果您未使用 RADIUS 預設通訊埠編號，您必須在防火牆上針對本機電腦設定例外狀況，以允許新埠上的 RADIUS 流量。 如需詳細資訊，請參閱[設定 RADIUS 流量的防火牆](nps-firewalls-configure.md)。

至少要有 **Domain Admins** 的成員資格或是對等成員資格，才能完成此程序。

## <a name="to-configure-nps-udp-port-information"></a>設定 NPS UDP 埠資訊 

1. 開啟 NPS 主控台。
2. 以滑鼠右鍵按一下 [**網路原則伺服器**]，**然後按一下 [** 內容]。
3. 按一下 [**埠**] 索引標籤，然後檢查 [埠] 的設定。 如果您的 RADIUS 驗證和 RADIUS 帳戶處理 UDP 埠與提供的預設值（1812和1645以進行驗證，而1813和1646適用于帳戶處理）不同，請在 [**驗證**和**帳戶**處理] 中輸入您的埠設定。
4. 驗證或帳戶處理要求若要使用多個連接埠設定，請以逗號分隔連接埠號碼。

如需管理 NPS 的詳細資訊，請參閱[管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。
