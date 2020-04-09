---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: 指派 AD DS 擁有者角色的 DNS
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8ef63fd9e53d20c812ff84e601e73d4276b304be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825261"
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>指派 AD DS 擁有者角色的 DNS

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

樹系擁有者會為樹系的 Active Directory Domain Services （AD DS）擁有者指派網域名稱系統（DNS）。 樹系 AD DS 擁有者的 DNS 是一員（或一群人），負責監督 AD DS 基礎結構的 DNS 部署，以及確定（如有必要）功能變數名稱已向適當的網際網路授權單位註冊。  
  
AD DS 擁有者的 DNS 負責 AD DS 設計樹系的 DNS。 如果您的組織目前正在操作 DNS 伺服器服務，現有 DNS 伺服器服務的 DNS 設計工具會與 AD DS 擁有者的 DNS 搭配運作，以將樹系根 DNS 名稱委派給在網域控制站上執行的 DNS 伺服器。  
  
樹系 AD DS 擁有者的 DNS 也會維護與組織的動態主機設定通訊協定（DHCP）群組和 DNS 群組的連絡人，並協調樹系中每個網域（如果有的話）與這些群組的個別 DNS 擁有者的計畫。 樹系的 DNS 擁有者可確保 DHCP 和 DNS 群組會與 DNS 中的 AD DS 設計程式相關，讓每個群組都知道 DNS 設計計畫，並且可以及早提供輸入。  
  


