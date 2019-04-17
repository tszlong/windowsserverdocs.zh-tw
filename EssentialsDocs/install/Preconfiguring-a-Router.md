---
title: "預先路由器"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9153ac90-bb0c-4b8d-93b2-e2121ed13636
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7dc66c8a439552c2087d0348b0115adba04027ee
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="preconfiguring-a-router"></a>預先路由器

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

一般而言，新的作業系統安裝需要網際網路適用的路由器，並防火牆客戶的連絡連接到網際網路。 如果您提供路由器額外的值為預先設定的伺服器，您可能需要額外的步驟來預先提供更好的使用者體驗路由器設定。  
  
 在路由器應該會有 DHCP 亮起來。 靜態 IP 位址應該指派伺服器。 您可以 DHCP 保留的 IP 位址或指派的 DHCP 位址範圍以外的 IP 位址。  
  
 您也應該預先連接埠轉送路由器上的設定將特定的連接埠路由器的外部介面從轉寄給上連絡伺服器的位址。 下表列出建議的設定。  
  
|設定|詳細資料|  
|---------------------------|-------------|  
|DHCP|在|  
|連接埠轉接|您應該將轉寄下列連接埠伺服器的位址：<br /><br /> -80（適用於裝載設定時，使用 443）<br />-   443|  
|UPnP 支援|您應該讓提供客戶及客戶的最佳使用體驗的最簡單路由器設定，在安裝期間 UPnP 度支援。<br /><br /> **警告：**架構 UPnP 如果尚未向左會安全性風險。|  
  
 除了基本路由器 preconfiguration 設定，您可以完成下列工作管理路由器提供整合式更多的使用者體驗：  
  
-   延長儀表板可讓使用者管理路由器透過自訂使用者介面的伺服器上提供的增益集。  
  
-   延伸健康警示，讓可以查看路由器的任何警示警示檢視器中。  
  
-   如果您的路由器支援多個子網路，必須手持伺服器的 IP 位址為透過 DHCP 一個 DNS 伺服器。  
  
-   如果路由器的作用中 DirectoryÂ® Domain Services 整合的存取控制功能，您就可以自動伺服器的初次設定時 Active Directory 整合。 您也應該公開路由器管理增益集儀表板中透過這項功能。  
  
> [!NOTE]
>  如需有關設定 wireless 連接，請[設定 Wireless 網路的支援](Configure-Support-for-a-Wireless-Network.md)。  
  
## <a name="see-also"></a>也了  
 [開始使用 Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)