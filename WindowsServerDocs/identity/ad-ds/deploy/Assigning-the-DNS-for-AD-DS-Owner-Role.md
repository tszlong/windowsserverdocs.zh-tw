---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: 指派 AD DS 擁有者角色的 DNS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dde9ed6035b30ba5b5b96b7132d25a1f8a1c0b8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884019"
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>指派 AD DS 擁有者角色的 DNS

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

樹系擁有者指派的 「 網域名稱系統 (DNS) 」，Active Directory 網域服務 (AD DS) 樹系的擁有者。 AD DS 樹系的擁有者的 DNS 是人員 （或一群人） 誰負責監督部署的 DNS 網域名稱的 AD DS 基礎結構以及確定 （如有必要） 會向適當的網際網路授權單位。  
  
AD DS 擁有者的 DNS 會負責樹系的 AD DS 設計的 DNS 項目。 如果您的組織目前正在 DNS 伺服器服務，現有的 DNS 伺服器服務的 DNS 設計工具適用於 AD DS 擁有者委派樹系根 DNS 名稱，在網域控制站上執行的 DNS 伺服器的 DNS。  
  
AD DS 樹系的擁有者的 DNS 也會連絡維護與動態主機設定通訊協定 (DHCP) 群組和組織的 DNS 群組，並協調這些群組與個別的樹系中的每個網域的 DNS 擁有者的方案 （如果有的話）。 樹系的 DNS 擁有者可確保 DHCP 和 DNS 的群組會涉及在 AD DS 設計程序的 DNS，讓每個群組會感知的 DNS 設計計劃，並可以及早提供輸入。  
  


