---
title: Windows Server core 網路指南
description: 本主題提供核心網路快速入門，可讓您計劃和部署核心元件正常運作的網路及新中新的 Windows Server 2016 的樹系的 Active Directory 網域時所需的概觀
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 9b3ef3eb-4246-4e0e-8bf1-53224ca5f2f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 63e4cf8c5bf56ef5131e835163a5fcb5dfd98b55
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide-for-windows-server"></a>Windows Server core 網路指南

>適用於：Windows Server、Windows Server 2016

本主題提供核心網路指南適用於 Windows Server 的概觀&reg;2016 年，包含下列各節。  
  
-   [Windows Server Core 網路簡介](#bkmk_intro)  
  
-   [Windows Server core 網路指南](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Windows Server Core 網路簡介

核心網路網路硬體、裝置的收藏，軟體提供的基本服務，您組織的資訊技術 (IT)，必須。

Windows Server core 網路為您提供許多好處，其中包括下列。

- 適用於電腦和其他傳輸控制項通訊協定日網際網路通訊協定 (TCP/IP) 的相容裝置之間網路連接核心通訊協定。 TCP/IP 是一套連接電腦以及建立網路標準通訊協定。 TCP/IP 是網路通訊協定軟體與 Microsoft 提供&reg;Windows&reg;，實作支援 TCP/IP 作業系統通訊協定的套件。

- 動態主機設定通訊協定」(DHCP) 伺服器自動 IP 位址。 手動所有網路上的電腦上的 IP 位址設定，而且費時彈性不如動態提供 DHCP 伺服器的 IP 位址租用電腦與其他裝置。

- 網域名稱系統」(DNS) 的名稱解析服務。 DNS 可讓使用者、電腦、應用程式與服務使用網域全名的電腦或裝置的網路上尋找電腦和裝置的 IP 位址。

- 森林，這是一或多個 Active Directory 網域分享相同課程和屬性定義（結構描述）、網站及複寫資訊（設定）和樹系的搜尋功能（通用）。

- 建立新的樹系森林根網域，也就是第一個的網域。 企業系統管理員和架構管理員群組的樹系管理群組，這位於森林根網域中。 此外，森林根網域，與其他網域，是電腦、使用者和群組物件 Active Directory Domain Services (AD DS) 中的系統管理員所定義的收藏。 這些物件共用常見 directory 資料庫和安全性原則。 它們也可以共用與其他網域安全關係，如果您為您的組織成長加入網域。 Directory 服務也會儲存 directory 資料，並允許授權的電腦、應用程式和使用者資料的存取。

- 使用者和電腦 account 資料庫。 Directory 服務提供可讓您的連絡人和電腦連接到您的網路及存取網路資源，例如應用程式、資料庫，共用的檔案和資料夾和印表機授權建立使用者和電腦帳號中央的使用者帳號資料庫。

核心網路也可讓您以調整您的網路，您組織的成長，以及變更 IT 需求。 例如，核心網路的問題，您可以新增網域、的 IP 子網路、遠端存取服務、wireless 服務，及其他功能及提供 Windows Server 2016 伺服器角色。

## <a name="bkmk_core"></a>Windows Server core 網路指南

Windows Server 2016 核心網路指南提供計劃和部署所需的正常運作的網路，以及新的 Active Directory 核心元件的方式指示&reg;在新的樹系的網域。 您可以使用此快速入門，部署電腦與 Windows server 元件下列設定：

- Active Directory Domain Services (AD DS) 伺服器角色

- 網域名稱系統」(DNS) 伺服器角色

- 「動態主機設定通訊協定」(DHCP) 伺服器角色

- 伺服器角色網路原則與服務存取權的網路原則 Server (NPS) 角色服務

- 網頁伺服器 (IIS) 伺服器角色

- 傳輸控制項通訊協定日網際網路通訊協定第 4 (TCP/IP) 個人伺服器連接情形

本指南可在下列位置。

- [核心網路指南](../core-network-guide/Core-Network-Guide.md)Windows Server 2016 技術文件庫中。
  


