---
title: 遠端桌面服務-高可用性
description: 規劃高可用性的 RDS 部署所設定的相關資訊。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec630ea0-ab80-4dfe-a25f-f4f601651f72
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: b5a2bd38c8831063d6fd2ba525b71a10403b8fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839259"
---
# <a name="remote-desktop-services---high-availability"></a>遠端桌面服務-高可用性

>適用於：Windows Server （半年通道），Windows Server 2016

失敗和節流會避免在大型系統中。 這很容易設定遠端桌面基礎結構角色，以支援高可用性，並讓使用者順暢地連接到每次。

在 遠端桌面服務中，下列項目代表具有其各自的指南，以建立高可用性的遠端桌面基礎結構角色：
- [遠端桌面連線代理人](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)
- [遠端桌面閘道](Deploy-a-RD-Web-Access-and-Gateway-farm.md)
- 遠端桌面授權
- [遠端桌面 Web 存取](Deploy-a-RD-Web-Access-and-Gateway-farm.md)

建立高可用性的方法是複製每個角色服務，在第二個的電腦上。 在 Azure 中，您可以收到運作時間保證，藉由將兩個虛擬機器 （裝載相同角色） 的集合放在可用性設定。

可用性設定組，以及您現在可以利用 Azure SQL Database 和其支援的 Azure SLA 的電源，以確保您一律有連接資訊，並可以將使用者重新導向至他們的桌上型電腦和應用程式。

如需建立您的 RDS 環境的最佳作法，請參閱[桌面主機架構](desktop-hosting-reference-architecture.md)。