---
title: Windows Server 核心網路指導方針
description: 本主題提供核心網路指南，可讓您計劃和部署全功能網路與 Windows Server 2016 的新樹系中的新 Active Directory 網域所需的核心元件的概觀
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 9b3ef3eb-4246-4e0e-8bf1-53224ca5f2f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a905fd0c11237edd3a408998f8f71aa25a054328
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847899"
---
# <a name="core-network-guidance-for-windows-server"></a>Windows Server 核心網路指導方針

>適用於：Windows Server、 Windows Server 2016

本主題提供適用於 Windows Server 核心網路指南概觀&reg;2016，且包含下列各節。  
  
-   [Windows Server 核心網路簡介](#bkmk_intro)  
  
-   [適用於 Windows Server 核心網路指南](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Windows Server 核心網路簡介

核心網路是網路硬體、裝置以及軟體的集合，提供組織資訊技術 (IT) 需求的基礎服務。

Windows Server 核心網路提供許多好處，包括下列各項。

- 電腦與其他傳輸控制通訊協定/網際網路通訊協定 (TCP/IP) 相容裝置之間網路連線能力的核心通訊協定。 TCP/IP 是一組標準的通訊協定，用於連線電腦與建置網路。 TCP/IP 是 Microsoft 所提供的網路通訊協定軟體&reg;Windows&reg;作業系統，實作和支援 TCP/IP 通訊協定套件。

- 動態主機設定通訊協定 (DHCP) 伺服器自動 IP 定址。 手動設定網路上所有電腦的 IP 位址，比從 DHCP 伺服器動態提供電腦及其他裝置使用的 IP 位址租用，更為耗時且缺乏彈性。

- 網域名稱系統 (DNS) 名稱解析服務。 DNS 允許使用者、電腦、應用程式及服務，使用電腦或裝置的完整網域名稱，尋找網路上電腦和裝置的 IP 位址。

- 樹系是一或多個 Active Directory 網域，它們共用相同類別與屬性定義 (架構)、站台與複寫資訊 (設定) 以及全樹系搜尋功能 (通用類別目錄)。

- 樹系根網域是在新樹系中建立的第一個網域。 Enterprise Admins 與 Schema Admins 群組是全樹系系統管理群組，位於樹系根網域中。 此外，樹系根網域如同其他網域一樣，都是系統管理員在 Active Directory 網域服務 (AD DS) 中定義之電腦、使用者及群組物件的集合。 這些物件共用一般目錄資料庫與安全性原則。 如果您隨著組織擴充而新增網域，它們也可以與其他網域共用安全性關係。 目錄服務也會儲存目錄資料，並允許授權的電腦、應用程式及使用者存取資料。

- 使用者與電腦帳戶資料庫。 目錄服務提供集中的使用者帳戶資料庫，可讓您為有權連線到您網路及存取網路資源 (例如應用程式、資料庫、共用檔案與資料夾及印表機) 的人及電腦建立使用者與電腦帳戶。

核心網路也可以讓您隨著組織擴充及 IT 需求變更來調整網路大小。 例如，使用核心網路，您可以加入網域、 IP 子網路、 遠端存取服務、 無線服務以及其他功能和 Windows Server 2016 所提供的伺服器角色。

## <a name="bkmk_core"></a>適用於 Windows Server 核心網路指南

Windows Server 2016 核心網路指南提供有關如何規劃和部署全功能網路與新的 Active Directory 所需的核心元件的指示&reg;新樹系中的網域。 使用本指南，您可以部署以下列 Windows 伺服器元件設定的電腦：

- Active Directory 網域服務 (AD DS) 伺服器角色

- 網域名稱系統 (DNS) 伺服器角色

- 動態主機設定通訊協定 (DHCP) 伺服器角色

- 網路原則與存取服務伺服器角色的網路原則伺服器 (NPS) 角色服務

- 網頁伺服器 (IIS) 伺服器角色

- 個別伺服器上的傳輸控制通訊協定/網際網路通訊協定第 4 版 (TCP/IP) 連線

本指南可在下列位置。

- [核心網路指南](../core-network-guide/Core-Network-Guide.md)在 Windows Server 2016 技術文件庫中。
  


