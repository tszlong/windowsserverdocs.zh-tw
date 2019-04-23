---
title: DNS 原則案例指南
description: 本主題是 DNS 原則案例指南的 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b3a1400a68e22fc4988c87c9222b66f718cf5fd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865639"
---
# <a name="dns-policy-scenario-guide"></a>DNS 原則案例指南

>適用於：Windows Server （半年通道），Windows Server 2016

本指南適合使用 DNS、 網路和系統管理員。  
  
DNS 原則是在 Windows Server 中的 DNS 新功能&reg;2016年。 您可以使用本指南以了解如何使用 DNS 原則來控制 DNS 伺服器如何處理根據您在原則中定義的不同參數的名稱解析查詢。   
  
本指南包含 DNS 原則概觀的資訊，以及特定的 DNS 原則案例，為您提供有關如何設定 DNS 伺服器行為的指示，完成您的目標，包括主要的地理位置流量管理和次要 DNS 伺服器、 應用程式高可用性、 拆分式 DNS，等等。  
  
本指南涵蓋下列各節。  
  
- [DNS 原則概觀](DNS-Policies-Overview.md)  
- [使用地理位置的 DNS 原則以透過主要伺服器的流量管理](primary-geo-location.md)  
- [對於地理位置為基礎的流量管理與主要-次要部署，使用 DNS 原則](primary-secondary-geo-location.md)  
- [使用 DNS 原則進行智慧型 DNS 回應的時間為基礎](dns-tod-intelligent.md)
- [DNS 回應為基礎的當日時間和 Azure 雲端應用程式伺服器](dns-tod-azure-cloud-app-server.md)
- [使用 DNS 原則進行拆分式 DNS 部署](split-brain-DNS-deployment.md)
- [使用 DNS 原則進行 Active Directory 中的拆分式 DNS](dns-sb-with-ad.md)
- [使用 DNS 原則進行 DNS 查詢上套用篩選器](apply-filters-on-dns-queries.md)
- [使用 DNS 原則進行應用程式負載平衡](app-lb.md)
- [使用 DNS 原則進行應用程式負載平衡與地理位置感知](app-lb-geo.md)

