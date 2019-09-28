---
title: 驗證伺服器憑證的伺服器註冊
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5f87db78d6f07d11c36193b1a56cf66bd44e7160
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356099"
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>驗證伺服器憑證的伺服器註冊

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式來確認您的網路原則伺服器（NPS）伺服器已向憑證授權單位單位（CA）註冊伺服器憑證。   
  
>[!NOTE]  
>若要完成這些程式，至少需要**Domain Admins**群組的成員資格。  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>確認伺服器憑證的網路原則伺服器（NPS）註冊  
  
由於 NPS 是用來驗證和授權網路連線要求，因此在網路原則中使用時，請務必確定您發行給 Nps 的伺服器憑證是有效的。  
  
若要確認伺服器憑證已正確設定並向 NPS 註冊，您必須設定測試網路原則，並允許 NPS 驗證 NPS 是否可以使用憑證進行驗證。  
  
### <a name="to-verify-nps-enrollment-of-a-server-certificate"></a>驗證伺服器憑證的 NPS 註冊  
  
1.  在伺服器管理員中，按一下 [**工具**]，然後按一下 [**網路原則伺服器**]。 [網路原則伺服器] Microsoft Management Console （MMC）隨即開啟。  
  
2.  按兩下 [**原則**]，以滑鼠右鍵按一下 [**網路原則**]，然後按一下 [**新增**]。 [新增網路原則] wizard 隨即開啟。  
  
3.  在 [**指定網路原則名稱和連線類型**] 的 [**原則名稱**] 中，輸入**測試原則**。 確定**網路存取伺服器的類型**具有**未指定**的值，然後按 **[下一步]** 。  
  
4.  在 [**指定條件**] 中，按一下 [**新增**]。 在 [**選取條件**] 中，按一下 [ **Windows 群組**]，然後按一下 [**新增**]。  
  
5.  在 [**群組**] 中，按一下 [**新增群組**]。 在 [**選取群組**] 中，輸入**Domain Users**，然後按 enter。 按一下 [確定]，然後按 [下一步]。  
  
6.  在 [**指定存取權限**] 中，確定已選取 [**授與存取**權]，然後按 **[下一步]** 。  
  
7.  在 [**設定驗證方法**] 中，按一下 [**新增**]。 在 [**新增 EAP**] 中，按一下 [**Microsoft]：受保護的 EAP （PEAP）** ，然後按一下 **[確定]** 。 在  **EAP 類型** 中，選取 **Microsoft：受保護的 EAP （PEAP）** ，然後按一下 **編輯**。 [**編輯受保護的 EAP 屬性**] 對話方塊隨即開啟。  
  
8.  在 [**編輯受保護的 EAP**內容] 對話方塊的 [**憑證發出至**] 中，NPS 會以*ComputerName*格式顯示伺服器憑證的名稱。*網域*。 例如，如果您的 NPS 命名為 NPS-01，而您的網域是 example.com，NPS 會顯示憑證**NPS-01.example.com**。 此外，在**簽發者**中，會顯示憑證授權單位單位的名稱，而且在**到期日**，會顯示伺服器憑證的到期日。 這會示範您的 NPS 已註冊有效的伺服器憑證，可用來向嘗試透過網路存取伺服器（例如虛擬私人網路（VPN）伺服器）存取網路的用戶端電腦證明其身分識別，802.1 X 功能無線存取點、遠端桌面閘道伺服器，以及 802.1 X 支援的乙太網路交換器。  
  
    > [!IMPORTANT]  
    > 如果 NPS 未顯示有效的伺服器憑證，而且如果它提供在本機電腦上找不到這類憑證的訊息，則此問題有兩個可能的原因。 群組原則可能未正確重新整理，而且 NPS 尚未向 CA 註冊憑證。 在此情況下，請重新開機 NPS。 當電腦重新開機時，會重新整理群組原則，而您可以再次執行此程式，以確認已註冊伺服器憑證。 如果重新整理群組原則無法解決此問題，可能是憑證範本、憑證自動註冊，或兩者都未正確設定。 若要解決這些問題，請從本指南的開頭開始，然後再次執行所有步驟，以確保您提供的設定正確無誤。  
  
9. 當您已驗證有效的伺服器憑證是否存在時，您可以按一下 **[確定] 和 [** **取消**]，結束 [新增網路原則] wizard。  
  
    > [!NOTE]  
    > 由於您未完成嚮導，因此不會在 NPS 中建立測試網路原則。  
  


