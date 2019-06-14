---
title: 預先設定路由器
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433506"
---
# <a name="preconfiguring-a-router"></a>預先設定路由器

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

一般來說，作業系統的全新安裝需要具備網際網路功能的路由器和防火牆，才能將客戶的內部網路連接至網際網路。 如果您隨預先設定伺服器提供路由器，以創造附加價值，您可以進一步預先設定路由器來提供更好的使用者經驗。  
  
 此路由器應該將 DHCP 開啟。 伺服器應獲指派靜態 IP 位址。 若要這麼做，您可以藉由 IP 位址的 DHCP 保留區，或指派 DHCP 位址範圍以外的 IP 位址。  
  
 您還必須在路由器上預先設定連接埠轉送設定，以將特定連接埠從路由器外部介面轉送至內部網路上的伺服器位址。 下表列出建議的設定。  
  
|組態設定|詳細資料|  
|---------------------------|-------------|  
|DHCP|開啟|  
|連接埠轉送|您應該將下列連接埠轉送至伺服器的位址：<br /><br /> -80 （適用於託管的設定，僅使用 443）<br />-   443|  
|UPnP 支援|您應該啟用 UPnP 狀況支援，以在安裝期間提供的最簡單的路由器設定客戶和最佳客戶體驗。<br /><br /> **警告：** 如果將 UPnP 架構保持啟用狀態，有可能會造成安全風險。|  
  
 除了基本路由器預先設定以外，您可以完成下列工作以便為管理路由器提供更加整合的使用者經驗：  
  
-   在能讓使用者能透過自訂使用者介面，來管理路由器的伺服器上提供增益集，擴充儀表板的功能。  
  
-   擴充健康狀態警示，以讓您可以在警示檢視器中查看來自路由器的任何警示。  
  
-   如果路由器可支援多個子網路，則必須透過 DHCP 將伺服器的 IP 位址當做一個 DNS 伺服器遞交。  
  
-   如果路由器擁有 Active DirectoryÂ® 網域服務的整合式的存取控制功能，您可以自動化的初始設定期間的 Active Directory 整合的伺服器。 您還必須透過儀表板中的路由器管理增益集來公開此功能。  
  
> [!NOTE]
>  如需有關設定無線連線的詳細資訊，請參閱 [Configure Support for a Wireless Network](Configure-Support-for-a-Wireless-Network.md)。  
  
## <a name="see-also"></a>另請參閱  
 [與 Windows Server Essentials ADK 快速入門](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)