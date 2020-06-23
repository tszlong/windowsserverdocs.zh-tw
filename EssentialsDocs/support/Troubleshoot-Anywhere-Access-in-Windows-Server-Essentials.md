---
title: Windows Server 2012 Essentials 中的隨處存取問題疑難排解
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 68f2b05c-09eb-4cba-8db4-a91353b513c6
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 313b4fae48a5b70cb16cae0cfb3a7bc048ef97a3
ms.sourcegitcommit: 56ac7cf3f4bbcc5175f140d2df5f37cc42ba76d1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85217558"
---
# <a name="troubleshoot-anywhere-access-in-windows-server-essentials"></a>Windows Server 2012 Essentials 中的隨處存取問題疑難排解

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本主題提供在 Windows Server Essentials 中使用「修復隨處存取嚮導」的一般指示，以疑難排解阻止網路使用者存取伺服器資源的問題。 隨處存取功能-遠端 Web 存取、虛擬私人網路（VPN）和 DirectAccess-讓網路使用者可以隨時從任何裝置，透過網際網路連線從任何位置存取伺服器資源。  
  
 「修復隨處存取」精靈會嘗試找出並修復路由器、網域名稱或防火牆阻止網路使用者從遠端存取伺服器資源的問題。  
  
> [!NOTE]
>  如需 Windows Server Essentials 社區最新的疑難排解資訊，建議您流覽[Windows Server Essentials 論壇](https://social.technet.microsoft.com/Forums/winserveressentials/threads)。 Windows Server Essentials 論壇是一個搜尋協助或詢問問題的好地方。  
  
### <a name="to-repair-anywhere-access"></a>修復隨處存取  
  
1.  登入伺服器，然後開啟儀表板。  
  
2.  按一下 [設定]****，然後按一下 [隨處存取]**** 索引標籤。  
  
3.  按一下 [修復]****。 此時會啟動「修復隨處存取」精靈。  
  
4.  按 [下一步] 。 精靈會分析隨處存取、識別問題，並嘗試修復問題。  
  
5.  如果您在精靈完成時收到警示，您可以按一下 [重試]**** 來重新嘗試修復問題。 如果您持續收到警示，請查看警示以了解問題和疑難排解步驟的其他相關資訊。  
  
### <a name="to-get-more-information-about-an-alert"></a>取得有關警示的詳細資訊  
  
1.  在儀表板右上角，按一下任何錯誤或警告圖示以開啟 [警示檢視器]。  
  
2.  在 [警示檢視器] 中，按一下錯誤或警告以檢視其他資訊。  
  
## <a name="additional-troubleshooting-for-anywhere-access"></a>隨處存取的其他疑難排解  
 如果「修復隨處存取」精靈無法修復隨處存取，請查看下列疑難排解資源瞭解遠端 Web 存取、VPN 和 DirectAccess 的相關問題：  
  

-   [遠端 Web 存取連線問題疑難排解](Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [疑難排解您的防火牆](Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)
  
-   查看[windows Server Essentials 論壇](https://social.technet.microsoft.com/Forums/winserveressentials/threads)，瞭解 Windows server essentials 社區回報的最新問題。
