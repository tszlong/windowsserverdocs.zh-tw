---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: 指派 AD DS 擁有者角色的 DNS
description: 為 AD DS 擁有者角色指派 DNS 的相關資訊。
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 56097e1a7db947be2d4b7dcbf83798f324346fc0
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068230"
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>指派 AD DS 擁有者角色的 DNS

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

樹系擁有者會為樹系的 Active Directory Domain Services (AD DS) 擁有者指派網域名稱系統 (DNS) 。 樹系 AD DS 擁有者的 DNS 是 (或人員群組的成員，) 負責監督 AD DS 基礎結構的 DNS 部署，以及確保 (必要的) 功能變數名稱已向適當的網際網路授權單位註冊。

AD DS 擁有者的 DNS 負責 AD DS 設計樹系的 DNS。 如果您的組織目前正在操作 DNS 伺服器服務，現有 DNS 伺服器服務的 DNS 設計工具就會與 AD DS 擁有者的 DNS 搭配運作，以將樹系根 DNS 名稱委派給網域控制站上執行的 DNS 伺服器。

樹系的 AD DS 擁有者的 DNS 也會與「動態主機設定通訊協定」（ (DHCP) 群組和組織的 DNS 群組）保持聯繫，並協調樹系中每個網域的個別 DNS 擁有者的規劃 (如果有這些群組的任何) 。 樹系的 DNS 擁有者可確保 DNS 中的 DHCP 和 DNS 群組 AD DS 設計程式，讓每個群組都知道 DNS 設計計畫，並可提早提供輸入。
