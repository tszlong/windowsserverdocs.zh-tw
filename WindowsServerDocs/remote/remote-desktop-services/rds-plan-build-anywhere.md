---
title: 遠端桌面服務 - 隨處建置
description: 規劃資訊可協助您判斷在哪裡裝載您的 RDS 部署。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: c803a383-0eea-4e11-bca5-d204ab758048
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 9b56614e347b36fb86e6e4680f1b179accaef058
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857371"
---
# <a name="remote-desktop-services---build-anywhere"></a>遠端桌面服務 - 隨處建置

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

在內部部署、在雲端或混合兩者部署。 隨著您的業務需求變更，修改您的部署。

無論您身在何處，遠端桌面服務環境的基礎[架構](desktop-hosting-logical-architecture.md)都保持不變：
- 您仍然必須有網際網路面向伺服器以便將 RD Web 存取和 RD 閘道用於外部使用者
- 您仍然必須有 Active Directory 以及適用於高可用性環境的 SQL 資料庫，才能裝置使用者和遠端桌面屬性
- 您仍然必須在 RD 基礎結構角色 (RD 連線代理人、RD 閘道、RD 授權以及 RD Web 存取) 與末端 RDSH 或 RDVH 主機之間擁有通訊存取權，才能夠將使用者連線到他們的桌面或應用程式。

這種彈性可讓您充分利用這兩方面：
- 與雲端和線上世界相關聯的簡單性和隨用隨付方法。
- 利用已經存在於內部部署的大量資源的熟悉度和無障礙方式。

如需其他資訊，請查看如何[建置和部署遠端桌面服務部署](rds-build-and-deploy.md)。