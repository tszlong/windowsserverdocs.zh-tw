---
title: 驗證伺服器憑證的伺服器註冊
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 45ba7a9a7fc5b9622ab1b9a94f38f4bf4de13192
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850899"
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>驗證伺服器憑證的伺服器註冊

>適用於：Windows Server （半年通道），Windows Server 2016

若要確認您的網路原則伺服器 (NPS) 伺服器已註冊伺服器憑證的憑證授權單位 (CA)，您可以使用此程序。   
  
>[!NOTE]  
>中的成員資格**Domain Admins**群組是要完成這些程序，至少需要。  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>確認伺服器憑證的網路原則伺服器 (NPS) 註冊  
  
因為 NPS 用來驗證和授權網路連線要求，務必確定您已發出至 NPSs 的伺服器憑證有效時用於網路原則。  
  
若要確認伺服器憑證已正確設定，且已註冊至 NPS，您必須設定測試網路原則，並允許 NPS 確認 NPS 可以使用憑證進行驗證。  
  
### <a name="to-verify-nps-enrollment-of-a-server-certificate"></a>若要確認 NPS 伺服器憑證的註冊  
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後按一下**網路原則伺服器**。 此時會開啟 網路原則伺服器 Microsoft Management Console (MMC)。  
  
2.  按兩下**原則**，以滑鼠右鍵按一下**網路原則**，然後按一下**新增**。 新的網路原則精靈 隨即開啟。  
  
3.  在 **指定的網路原則名稱和連接類型**，請在**原則名稱**，型別**測試原則**。 請確認**網路存取伺服器類型**具有值**Unspecified**，然後按一下 [**下一步]**。  
  
4.  在 **指定條件**，按一下**新增**。 在 **選取條件**，按一下**Windows 群組**，然後按一下**新增**。  
  
5.  在 **群組**，按一下**新增群組**。 在 **選取群組**，型別**網域使用者**，然後按 ENTER 鍵。 按一下 [確定]，然後按 [下一步]。  
  
6.  在 [**指定的存取權限**，請確認**授與存取權**已選取，然後按一下**下一步]**。  
  
7.  在 **設定驗證方法**，按一下**新增**。 在 **新增 EAP**，按一下  **Microsoft:受保護的 EAP (PEAP)**，然後按一下**確定**。 在  **EAP 類型**，選取**Microsoft:受保護的 EAP (PEAP)**，然後按一下**編輯**。 **編輯受保護的 EAP 內容**對話方塊隨即開啟。  
  
8.  在**編輯受保護的 EAP 內容**對話方塊中，於**發給憑證**，NPS 會顯示您的伺服器憑證名稱格式*ComputerName*。*網域*。 例如，您的 NPS 名為為 nps-01，而且您的網域是 example.com，NPS 會顯示憑證**NPS 01.example.com**。 此外，在**簽發者**，會顯示您的憑證授權單位的名稱，然後在**到期日**，會顯示伺服器憑證的到期日期。 這示範了您的 NPS 已註冊有效的伺服器憑證可供它嘗試透過網路存取伺服器，例如虛擬私人網路 (VPN) 伺服器、 802.1 存取網路的用戶端電腦證明其識別身分支援無線存取點、 遠端桌面閘道伺服器和 802.1 X 的乙太網路交換器。  
  
    > [!IMPORTANT]  
    > 如果 NPS 不會顯示有效的伺服器憑證，而且它會提供在本機電腦找不到這類憑證的訊息，有兩個可能的原因，此問題。 它是群組原則重新整理正常運作，而且 NPS 尚未註冊來自 CA 的憑證。 在此情況下，重新啟動 NPS。 當電腦重新啟動時，重新整理群組原則時，以及您可以執行此程序，以確認已註冊的伺服器憑證。 如果重新整理群組原則不會解決此問題，憑證範本、 憑證自動註冊，或兩者未正確設定。 若要解決這些問題，本指南的開頭開始，並執行所有步驟，以確保您所提供的設定正確。  
  
9. 當您已有有效的伺服器憑證時，您可以按一下 **[確定]** 並**取消**結束 [新增網路原則精靈]。  
  
    > [!NOTE]  
    > 因為您未完成精靈，在 NPS 中將不會建立測試網路原則。  
  


