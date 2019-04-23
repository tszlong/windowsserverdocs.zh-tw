---
title: 遠端桌面服務的任何位置的組建
description: 若要協助您判斷如何裝載您的 RDS 部署的規劃資訊。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c803a383-0eea-4e11-bca5-d204ab758048
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: cbb8e73d753b1fe4f0293cf4427c634020a23a42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869509"
---
# <a name="remote-desktop-services---build-anywhere"></a>遠端桌面服務的任何位置的組建

>適用於：Windows Server （半年通道），Windows Server 2016

內部部署，在雲端或兩者的混合式。 隨著您的業務需求變更，請修改您的部署。

無論您身在何處，基礎[架構](desktop-hosting-logical-architecture.md)的遠端桌面服務環境保持不變：
- 您仍然必須有外部使用者使用 RD Web 存取和 RD 閘道的網際網路對向伺服器
- 您仍必須擁有 Active Directory 和--針對高可用性環境使用，SQL database 房屋使用者 」 和 「 遠端桌面的屬性
- 您仍必須擁有 RD 基礎結構角色 （RD 連線代理人、 RD 閘道、 RD 授權以及 RD Web 存取） 和結束 RDSH 之間的通訊存取或 RDVH 主機能夠連線到他們的桌面或應用程式的使用者。

這種彈性可讓您取得之兩個優點：
- 線上世界與雲端相關聯的簡易性和隨用隨付方法。
- 熟悉度和麻煩的方法，運用沈重的資源已存在於內部部署。

如需詳細資訊，看看如何[建置和部署您的遠端桌面服務部署](rds-build-and-deploy.md)。