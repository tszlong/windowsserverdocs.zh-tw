---
title: 疑難排解 Windows Server Essentials 的防火牆
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6764645182a6f31c0aec4b5d9b9963d64500ea9a
ms.sourcegitcommit: 56ac7cf3f4bbcc5175f140d2df5f37cc42ba76d1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85217438"
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>疑難排解 Windows Server Essentials 的防火牆
 
>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials
  
 如果您遇到遠端存取問題，請執行 [修復隨處存取精靈]。  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>執行 [修復隨處存取精靈]  
  
1. 開啟 [儀表板]。  
  
2. 按一下 [設定]****，再按一下 [隨處存取]**** 索引標籤，然後按一下 [修復]****。  
  
3. 請遵循 [修復隨處存取精靈] 中的指示。  
  
   如果您使用進階的網路安裝程式或使用非 Microsoft 防火牆，您可能需要在防火牆上開啟其他連接埠。 下表中的連接埠已向 Internet Assigned Numbers Authority (IANA) 註冊。  
  
|連接埠號碼|描述|  
|-----------------|-----------------|  
|65500|憑證 Web 服務|  
|65510 和 65515|用戶端電腦部署網站|  
|65520|Mac 用戶端電腦的 Web 服務|  
|65532|伺服器回送通訊的提供者架構|  
|6602|伺服器與用戶端電腦之間通訊的提供者架構|  
  
## <a name="see-also"></a>另請參閱  
  
-   [使用遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理遠端 Web 存取](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理隨處存取](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  

-   [支援 Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

