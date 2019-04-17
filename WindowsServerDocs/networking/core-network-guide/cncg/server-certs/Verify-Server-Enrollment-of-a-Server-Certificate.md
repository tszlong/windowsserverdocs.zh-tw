---
title: 請確認伺服器註冊伺服器的憑證
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d8ff51fa83972e2fc73ee54628eeb89e2927046d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>請確認伺服器註冊伺服器的憑證

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序，以確認您的網路原則 Server (NPS) 伺服器的已退出伺服器的憑證授權單位憑證。   
  
>[!NOTE]  
>資格在**網域系統管理員**群組最小，才能完成這些程序。  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>請確認伺服器的憑證的網路原則 Server (NPS) 註冊  
  
NPS 來驗證，以及授權網路連接要求，因為很重要，以確保您具有 NPS 伺服器核發伺服器憑證搭配使用的網路原則。  
  
若要驗證伺服器的憑證已正確設定，並會 NPS 伺服器退出，您必須設定測試網路原則，並允許 NPS 驗證 NPS，可以使用憑證的驗證。  
  
### <a name="to-verify-nps-server-enrollment-of-a-server-certificate"></a>若要確認 NPS 伺服器註冊伺服器的憑證  
  
1.  在伺服器管理員中，按一下 [**工具**，然後按**的網路原則伺服器**。 開啟網路原則伺服器 Microsoft Management Console (MMC)。  
  
2.  按兩下**原則**，以滑鼠右鍵按一下**的網路原則**，並按一下 [**新**。 [新的網路原則精靈開啟。  
  
3.  在**網路指定原則名稱，並連接輸入**，請在**原則的名稱**，輸入**測試原則**。 確保**類型的網路存取伺服器**的值**未指定**，然後按一下 [**下一步**。  
  
4.  在**指定條件**，按一下 [**新增]**。 在**選取條件**，按一下 [ **Windows 群組**，然後按一下 [**新增**。  
  
5.  在**群組**，按一下 [**新增群組**。 在**選取的群組**，輸入**使用者網域**，然後按 ENTER 鍵。 按一下**[確定]**，然後按一下 [**下**。  
  
6.  在**指定的存取權限**，確認**存取授與**已選取，然後按一下 [**下一步**。  
  
7.  在**設定的驗證方法**，按一下 [**新增]**。 在**新增 EAP**，按一下 [ **Microsoft： 保護 EAP (PEAP)**，然後按一下 [ **[確定]**。 在**Eap**，請選取**Microsoft： 保護 EAP (PEAP)**，然後按一下 [**編輯**。 **編輯保護 EAP 屬性**對話方塊。  
  
8.  在**編輯保護 EAP 屬性**對話方塊中，在**發給**、 NPS 顯示您伺服器的憑證的名稱的格式*電腦名稱*。*網域*。 例如，如果 NPS 伺服器稱為 NPS-01 且您的網域 example.com，NPS 會顯示憑證**NPS-01.example.com**。此外，在**發行者**，會顯示您憑證授權單位的名稱，並在**到期**，顯示伺服器的憑證的到期日期。 這示範 NPS 伺服器的已退出有效的伺服器的憑證，它可以使用其身份 client 電腦嘗試存取網路透過您的網路存取伺服器，例如私人網路 virtual (VPN) 伺服器、 802.1 X 能力 wireless 存取點、 遠端桌面閘道伺服器及 802.1 X 能力乙太網路切換。  
  
    > [!IMPORTANT]  
    > 如果 NPS 不會顯示有效的伺服器的憑證，並提供本機電腦找不到憑證這類的訊息，如果有兩個可能的原因，此問題。 它可能會的群組原則不正常運作，更新並不 NPS 伺服器已退出從 CA 憑證。 在這個情況，重新開機 NPS 伺服器。 當您在電腦重新開機時，群組原則的更新，，以及您可以執行此程序，確認已退出的伺服器的憑證。 重新整理群組原則不解析這個問題，若憑證範本、 認證自動授權，或兩者未設定正確。 若要修正這些問題的相關，本文開頭的 [開始] 並執行所有步驟再試一次，以確保您所提供的設定正確。  
  
9. 當您已驗證的有效的伺服器的憑證時，您可以按一下**[確定]**和**取消**以結束新的網路原則精靈。  
  
    > [!NOTE]  
    > 因為您未完成精靈，測試的網路原則不會建立 NPS。  
  


