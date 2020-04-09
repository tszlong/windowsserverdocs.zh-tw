---
title: 預先設定路由器
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 9153ac90-bb0c-4b8d-93b2-e2121ed13636
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c39bf3ac260a23b7fc9cc9feec7f34786b1e8aae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819941"
---
# <a name="preconfiguring-a-router"></a>預先設定路由器

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

一般來說，作業系統的全新安裝需要具備網際網路功能的路由器和防火牆，才能將客戶的內部網路連接至網際網路。 如果您隨預先設定伺服器提供路由器，以創造附加價值，您可以進一步預先設定路由器來提供更好的使用者經驗。  
  
 此路由器應該將 DHCP 開啟。 伺服器應獲指派靜態 IP 位址。 若要這麼做，您可以藉由 IP 位址的 DHCP 保留區，或指派 DHCP 位址範圍以外的 IP 位址。  
  
 您還必須在路由器上預先設定連接埠轉送設定，以將特定連接埠從路由器外部介面轉送至內部網路上的伺服器位址。 下表列出建議的設定。  
  
|組態設定|詳細資料|  
|---------------------------|-------------|  
|DHCP|開啟|  
|連接埠轉送|您應該將下列連接埠轉送至伺服器的位址：<br /><br /> -80 （針對託管設定，只使用443）<br />-443|  
|UPnP 支援|您應該啟用 UPnP 支援，為客戶提供最簡單的路由器設定，並在安裝期間獲得最佳的客戶體驗。<br /><br /> **警告：** 如果將 UPnP 架構保持啟用狀態，可能會造成安全性風險。|  
  
 除了基本路由器預先設定以外，您可以完成下列工作以便為管理路由器提供更加整合的使用者經驗：  
  
-   在能讓使用者能透過自訂使用者介面，來管理路由器的伺服器上提供增益集，擴充儀表板的功能。  
  
-   擴充健康狀態警示，以讓您可以在警示檢視器中查看來自路由器的任何警示。  
  
-   如果路由器可支援多個子網路，則必須透過 DHCP 將伺服器的 IP 位址當做一個 DNS 伺服器遞交。  
  
-   如果路由器具有 Active DirectoryÂ&reg; 網域服務的整合式存取控制功能，您可以在伺服器的初始設定期間自動化 Active Directory 整合。 您還必須透過儀表板中的路由器管理增益集來公開此功能。  
  
> [!NOTE]
>  如需設定無線連線的詳細資訊，請參閱[設定無線網路支援](Configure-Support-for-a-Wireless-Network.md)。  
  
## <a name="see-also"></a>另請參閱  
 [消費者入門與 Windows Server ESSENTIALS ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)   
 [其他自訂](Additional-Customizations.md)   
 [準備映射以進行部署](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)