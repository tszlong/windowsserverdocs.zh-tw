---
title: "設定 Wireless 網路的支援"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d7020d4-fd46-4858-a406-de5c0f21ea06
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c5c98727b81bf37fdb3f90c612270462a51908c8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="configure-support-for-a-wireless-network"></a>設定 Wireless 網路的支援

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以設定 wireless 網路的支援的作業系統。 以便 wireless 伺服器上的支援，必須符合下列需求：  
  
-   伺服器必須安裝有線的網路介面卡。  
  
-   如果不支援的作業系統的網路介面卡必須預先安裝正確的驅動程式 wireless 網路介面卡。  
  
-   必須開放讓和停用 wireless 網路介面卡的能力。 這樣的方法可能會包含儀表板中的上伺服器或自訂使用者介面的實體按鈕。 Product 手動應該提供讓和停用 wireless 網路介面卡的步驟。  
  
-   必須開放選取 wireless 網路，以將它連接的能力。 這應該來將自訂使用者介面新增至儀表板。 Product 手動應該提供的步驟進行選取及連接 wireless 網路。  
  
-   如果需要可支援 wireless 特定網路，必須提供延伸的使用者介面中儀表板。 在使用者介面可以按鈕或時限設定 Windows Server 2008 R2 在控制台中無線特定網路精靈的連結。  
  
## <a name="additional-considerations"></a>其他注意事項  
 設定 wireless 網路的支援時，也應該被視為下列資訊：  
  
-   伺服器必須連接到花朵執行安裝程式與網路。  
  
-   網路並具有花朵必須連接網路執行極金屬還原電腦。  
  
-   伺服器必須連接的花朵執行極金屬還原伺服器的網路。  
  
-   如果建立伺服器上的特定網路，讓使用者必須一律纜網路伺服器以取得網際網路連接到專用 wireless 網路介面卡的特定網路。  
  
> [!NOTE]
>  如需有關設定網路的連接，請[預先路由器](Preconfiguring-a-Router.md)。  
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)