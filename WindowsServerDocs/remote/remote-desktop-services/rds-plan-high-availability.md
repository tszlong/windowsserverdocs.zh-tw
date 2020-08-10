---
title: 遠端桌面服務 - 高可用性
description: 設定高可用性 RDS 部署的相關規劃資訊。
ms.topic: article
ms.assetid: ec630ea0-ab80-4dfe-a25f-f4f601651f72
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: c50c66bee8f9f387497ef2f5971dbee5e1b402c6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955005"
---
# <a name="remote-desktop-services---high-availability"></a>遠端桌面服務 - 高可用性

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

在大規模的系統中，失敗和節流是難以避免的。 您可以輕鬆設定遠端桌面基礎結構角色以支援高可用性，並且讓使用者每次都能順暢地連線。

在遠端桌面服務中，下列項目代表遠端桌面基礎結構角色，且分別可依循其各自的指南建立高可用性：
- [遠端桌面連線代理人](./rds-connection-broker-cluster.md)
- [遠端桌面閘道](./rds-rdweb-gateway-ha.md)
- 遠端桌面授權
- [遠端桌面 Web 存取](./rds-rdweb-gateway-ha.md)

建立高可用性的方法是將每個角色服務複製到第二個機器上。 在 Azure 中，您只要將兩個一組的虛擬機器 (託管相同角色) 放在可用性設定組中，就可以有可預期的運作時間。

除了可用性設定組，現在您也可以搭配使用 Azure SQL Database 及其由 Azure 支援的 SLA，確保您一律會有連線資訊，並且可將使用者重新導向至其桌面和應用程式。

如需建立 RDS 環境的最佳做法，請參閱[桌面託管架構](desktop-hosting-reference-architecture.md)。
