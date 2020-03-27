---
title: 設定無線網路支援
description: 說明如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d7020d4-fd46-4858-a406-de5c0f21ea06
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4ce5f167339d7910b10f90bea5bbeae5606b5cf2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312209"
---
# <a name="configure-support-for-a-wireless-network"></a>設定無線網路支援

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以設定作業系統以支援無線網路。 必須符合下列需求，才能在伺服器上啟用無線支援：  
  
-   伺服器必須已安裝有線的網路配接器。  
  
-   如果作業系統不支援網路配接器，則必須為無線網路配接器預先安裝正確的驅動程式。  
  
-   必須提供啟用和停用無線網路配接器的功能。 要這麼做的方法可能包括在伺服器上使用實體按鈕，或在儀表板中使用自訂的使用者介面。 產品手冊應提供啟用和停用無線網路配接器的步驟。  
  
-   必須提供選取無線網路及連接無線網路的功能。 您應該透過將自訂使用者介面加入至儀表板，來達到此目的。 產品手冊應提供選取及連接至無線網路的相關步驟。  
  
-   如果需要支援無線臨機操作網路的功能，則必須在儀表板中提供擴充的使用者介面。 此使用者介面可以是一個按鈕或連結，能夠啟動 Windows Server 2008 R2 的控制台中的「設定無線臨機操作網路精靈」。  
  
## <a name="additional-considerations"></a>其他考量  
 設定無線網路支援時，也必須考量下列資訊：  
  
-   伺服器必須以有線方式連接至網路，才能執行設定。  
  
-   執行裸機還原的網路電腦必須以有線方式連接至網路。  
  
-   伺服器必須以有線方式連接至網路，才能執行伺服器的裸機還原。  
  
-   如果臨機操作網路是建立於伺服器上，則無線網路配接器專供臨機操作網路使用，所以使用者必須永遠將網路線接在伺服器上，以獲得網際網路連線。  
  
> [!NOTE]
>  如需設定網路連線的詳細資訊，請參閱[預先設定路由器](Preconfiguring-a-Router.md)。  
  
## <a name="see-also"></a>另請參閱  
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)   
 [其他自訂](Additional-Customizations.md)   
 [準備映射以進行部署](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)