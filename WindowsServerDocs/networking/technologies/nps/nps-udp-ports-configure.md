---
title: 設定 NPS UDP 連接埠資訊
description: 若要設定的網路原則 Server (NPS) 使用驗證遠端驗證 Dial 使用者服務 (RADIUS) 與 Windows Server 2016 中的計量流量的連接埠，您可以使用此主題。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f0e703dc6f9083f1e79091a6cee6d1ac58753d12
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-udp-port-information"></a>設定 NPS UDP 連接埠資訊

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用下列程序，設定的網路原則 Server (NPS) 使用遠端驗證撥號使用者服務 \(RADIUS\) 驗證及計量流量的連接埠。

根據預設，NPS RADIUS 連接埠上流量 1812 年、1813 年，1645 年和網際網路通訊協定第 6 版 \(IPv6\) 和 IPv4 1646 所有已安裝的網路介面卡接聽。

>[!NOTE]
>如果您解除安裝 IPv4 或 IPv6 網路介面卡、NPS 不會監視 RADIUS 傳輸通訊協定解除安裝。

連接埠值驗證 1812 年和 1813，用於會計是透過網際網路工程設計工作人員 \(IETF\) Rfc 2865 和 2866 年定義 RADIUS 標準連接埠。 不過，預設許多存取伺服器用於連接埠 1645 驗證需求和 1646 年計量要求。 不論您要使用的連接埠號碼，請確定 NPS 及存取伺服器設定要使用相同的。

>[重要]如果您不使用 RADIUS 預設連接埠號碼，您必須設定例外防火牆允許上新的連接埠 RADIUS 流量本機電腦上。 如需詳細資訊，請查看[設定防火牆 RADIUS 流量的](nps-firewalls-configure.md)。

資格在**網域系統管理員**，或相當於，才能完成此程序最小值。

## <a name="to-configure-nps-udp-port-information"></a>若要設定 NPS UDP 連接埠資訊 

1. 打開 NPS 主機。
2. 以滑鼠右鍵按一下**的網路原則伺服器**，然後按一下 [**屬性**。
3. 按一下**連接埠**索引標籤，然後檢查 [連接埠的設定。 如果您 RADIUS 驗證與 RADIUS 計量 UDP 連接埠不同的預設值，提供（1812 年 1645 進行驗證，以及 1813 年及 1646，用於會計），輸入連接埠設定在**驗證**和**計量**。
4. 驗證或計量要求使用多個連接埠設定，來以逗號分隔連接埠號碼。

如需有關管理 NPS 的詳細資訊，請查看[管理的網路原則伺服器]](nps-manage-top.md)。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。
