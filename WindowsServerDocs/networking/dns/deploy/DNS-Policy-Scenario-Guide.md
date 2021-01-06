---
title: DNS 原則案例指南
description: 瞭解如何使用 DNS 原則來控制 DNS 伺服器如何根據您在原則中定義的不同參數來處理名稱解析查詢。
manager: brianlic
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 49a928ab46bc190af279b17eaa23ca0ae04869b5
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904623"
---
# <a name="dns-policy-scenario-guide"></a>DNS 原則案例指南

>適用於：Windows Server (半年度管道)、Windows Server 2016

本指南旨在供 DNS、網路和系統管理員使用。

DNS 原則是 Windows Server 2016 中 DNS 的新功能 &reg; 。 您可以使用本指南來瞭解如何使用 DNS 原則來控制 DNS 伺服器如何根據您在原則中定義的不同參數來處理名稱解析查詢。

本指南包含 DNS 原則總覽資訊，以及可提供您如何設定 DNS 伺服器行為以達成目標的指示，包括主要和次要 DNS 伺服器的地理位置型流量管理、應用程式高可用性、分裂式 DNS 等等。

本指南涵蓋下列各節。

- [DNS 原則總覽](DNS-Policies-Overview.md)
- [透過主要伺服器使用地理位置流量管理的 DNS 原則](primary-geo-location.md)
- [透過主要-次要部署使用地理位置流量管理的 DNS 原則](primary-secondary-geo-location.md)
- [使用 DNS 原則以時間為基礎進行智慧型 DNS 回應](dns-tod-intelligent.md)
- [以 Azure Cloud App Server 的當日時間為基礎的 DNS 回應](dns-tod-azure-cloud-app-server.md)
- [使用 DNS 原則進行 Split-Brain DNS 部署](split-brain-DNS-deployment.md)
- [在 Active Directory 中使用 Split-Brain DNS 的 DNS 原則](dns-sb-with-ad.md)
- [使用 DNS 原則在 DNS 查詢上套用篩選](apply-filters-on-dns-queries.md)
- [使用 DNS 原則進行應用程式負載平衡](app-lb.md)
- [使用 DNS 原則進行具有地理位置感知的應用程式負載平衡](app-lb-geo.md)

