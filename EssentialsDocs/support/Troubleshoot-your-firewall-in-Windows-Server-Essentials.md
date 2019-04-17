---
title: "在 Windows Server Essentials 防火牆的疑難排解"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3c48d2abb7fd8431f40f76f8eece5c4142be4c75
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>在 Windows Server Essentials 防火牆的疑難排解
 
>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
  
 如果您遇到遠端存取執行修復任何地方存取精靈中的問題。  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>若要執行修復任何地方存取精靈  
  
1.  打開儀表板。  
  
2.  按一下**設定**，按一下 [**隨處存取**索引標籤，然後按一下 [**修復**。  
  
3.  請依照精靈中的指示修復隨處存取。  
  
 如果您是使用網路進階的設定，或使用非 Microsoft 防火牆，您可能需要開放防火牆額外的連接埠。 下表中的連接埠的登記與網際網路受指派的數字授權 (IANA)。  
  
|連接埠號碼|描述|  
|-----------------|-----------------|  
|65500|憑證 web 服務|  
|65510 和 65515|Client 電腦部署網站|  
|65520|Mac client 電腦 web 服務|  
|65532|提供者的伺服器回送通訊的架構|  
|6602|提供者的伺服器和 client 電腦間通訊架構|  
  
## <a name="see-also"></a>也了  
  
-   [使用遠端存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理網路遠端存取](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理隨時隨地存取](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Windows Server Essentials 的支援](Support-Windows-Server-Essentials.md)

-   [Windows Server Essentials 的支援](../support/Support-Windows-Server-Essentials.md)

