---
title: DNS 原則案例指南
description: 本主題是 Windows Server 2016 DNS 原則案例指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2168bc6d2f2b3a5f365bb2738a15ce7f96408de2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317925"
---
# <a name="dns-policy-scenario-guide"></a>DNS 原則案例指南

>適用於：Windows Server (半年通道)、Windows Server 2016

本指南旨在供 DNS、網路和系統管理員使用。  
  
DNS 原則是 Windows Server&reg; 2016 中 DNS 的新功能。 您可以使用本指南來瞭解如何使用 DNS 原則來控制 DNS 伺服器如何根據您在原則中定義的不同參數來處理名稱解析查詢。   
  
本指南包含 DNS 原則總覽資訊，以及特定的 DNS 原則案例，其中提供如何設定 DNS 伺服器行為以達成目標的指示，包括主要和的地理位置型流量管理次要 DNS 伺服器、應用程式高可用性、分裂式 DNS 等等。  
  
本指南涵蓋下列各節。  
  
- [DNS 原則總覽](DNS-Policies-Overview.md)  
- [將 DNS 原則用於以地理位置為基礎的流量管理與主伺服器](primary-geo-location.md)  
- [針對以地理位置為基礎的流量管理，使用主要-次要部署的 DNS 原則](primary-secondary-geo-location.md)  
- [根據當日時間使用 DNS 原則來進行智慧型 DNS 回應](dns-tod-intelligent.md)
- [使用 Azure 雲端應用程式伺服器以一天的時間為基礎的 DNS 回應](dns-tod-azure-cloud-app-server.md)
- [使用 DNS 原則進行分割的 DNS 部署](split-brain-DNS-deployment.md)
- [在 Active Directory 中使用適用于分裂式 DNS 的 DNS 原則](dns-sb-with-ad.md)
- [使用 DNS 原則在 DNS 查詢上套用篩選](apply-filters-on-dns-queries.md)
- [使用 DNS 原則進行應用程式負載平衡](app-lb.md)
- [使用 DNS 原則進行應用程式負載平衡與地理位置感知](app-lb-geo.md)

