---
title: 依據服務連接點設定自動託管快取探索
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 電腦上的託管快取模式部署 BranchCache 的指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd77fc76a999517cb8372aec8dfad25b4dd5be3b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829719"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>依據服務連接點設定自動託管快取探索

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

此程序可以使用群組原則來啟用，並在網域上設定 BranchCache 託管快取模式\-加入的電腦執行下列 BranchCache\-支援 Windows 作業系統。

- Windows 10 企業版
- Windows 10 教育版
- Windows 8.1 Enterprise
- Windows 8 企業版

> [!NOTE]  
> 若要設定執行 Windows Server 2008 R2 或 Windows 7 的加入網域的電腦，請參閱 Windows Server 2008 R2 [BranchCache 部署指南](https://technet.microsoft.com/library/ee649232.aspx)。

若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>若要使用群組原則來設定託管快取模式用戶端

1. Active Directory 網域服務伺服器角色已安裝的電腦，開啟 伺服器管理員，選取 本機伺服器，按一下**工具**，然後按一下**群組原則管理**。 群組原則管理主控台隨即開啟。

2. 在 [群組原則管理] 主控台中，展開下列路徑：**樹系：** *corp.contoso.com*，**網域**， *corp.contoso.com*，**群組原則物件**，其中*corp.contoso.com*是您想要設定 BranchCache 用戶端電腦帳戶所在網域的名稱。

3. 右\-按一下 **群組原則物件**，然後按一下**新增**。 **新的 GPO**對話方塊隨即開啟。 在 **名稱**，輸入新的群組原則物件的名稱\(GPO\)。 例如，如果您想要命名的物件 BranchCache 用戶端電腦，輸入**BranchCache 用戶端電腦**。 按一下 [確定] 。

4. 在 [群組原則管理] 主控台中，確定**群組原則物件**已選取，在 [詳細資料] 窗格右邊\-按一下您剛才建立的 GPO。 例如，如果您名為 GPO BranchCache 用戶端電腦，以滑鼠右鍵\-按一下  **BranchCache 用戶端電腦**。 按一下 **\[編輯\]**。 群組原則管理編輯器主控台隨即開啟。

5. 在 [群組原則管理編輯器] 主控台中，展開下列路徑：**電腦設定**，**原則**，**系統管理範本：原則定義\(ADMX 檔案\)從本機電腦擷取**，**網路**， **BranchCache**。

6. 按一下  **BranchCache**，然後在 詳細資料 窗格中，連按兩下\-按一下 **開啟 BranchCache**。 **開啟 BranchCache**對話方塊隨即開啟。
  
7.  在 [**開啟 BranchCache** ] 對話方塊中，按一下**已啟用**，然後按一下 **[確定]**。

8. 在 [群組原則管理編輯器] 主控台中，確定**BranchCache**是仍在選取狀態，然後在詳細資料窗格雙精度浮點數\-按一下**啟用自動託管快取探索的服務連線點**。 原則的 [設定] 對話方塊隨即開啟。

9. 在 **啟用自動託管快取探索服務連接點所** 對話方塊中，按一下**已啟用**，然後按一下  **確定**。

10. 若要啟用 BranchCache 的檔案伺服器的用戶端電腦下載並快取內容\-型內容伺服器：在 群組原則管理編輯器 主控台中，確定**BranchCache**是仍在選取狀態，然後在詳細資料窗格雙精度浮點數\-按一下 **網路檔案的 BranchCache**。 **設定網路檔案的 BranchCache**對話方塊隨即開啟。 
11. 在 [**設定網路檔案的 BranchCache** ] 對話方塊中，按一下**已啟用**。 在 **選項**，輸入數值，以毫秒為單位，最大的來回網路延遲時間，然後按一下 **確定**。
  
    > [!NOTE]
    > 根據預設，用戶端電腦快取從檔案伺服器的內容，如果超過 80 毫秒的來回網路延遲。
  
12. 若要設定的 BranchCache 快取的每個用戶端電腦上配置的硬碟空間量：在 [群組原則管理編輯器] 主控台中，確定**BranchCache**是仍在選取狀態，然後在詳細資料窗格雙精度浮點數\-按一下**設定的用戶端電腦快取所使用的磁碟空間百分比**. **設定的用戶端電腦快取所使用的磁碟空間百分比**對話方塊隨即開啟。 按一下  **Enabled**，然後在**選項**輸入數值，表示每個用戶端電腦上用於 BranchCache 快取的硬碟空間的百分比。 按一下 [確定] 。

13. 若要指定預設的存留期，以天為單位，為其區段在中都有效用戶端電腦上的 BranchCache 資料快取：在 群組原則管理編輯器 主控台中，確定**BranchCache**是仍在選取狀態，然後在詳細資料窗格雙精度浮點數\-按一下 **中的資料快取設定的區段存留期**。 **中的資料快取設定的區段存留期**對話方塊隨即開啟。 按一下 **已啟用**，然後在 詳細資料 窗格中輸入您偏好的日數。 按一下 [確定] 。

14. 設定用戶端電腦的其他 BranchCache 原則設定，視您的部署。

15. 分公司辦公室用戶端電腦上重新整理群組原則，執行命令**gpupdate /force**，或重新啟動用戶端電腦。

BranchCache 託管快取模式部署現在已完成的。

如需其他有關本指南中的技術的詳細資訊，請參閱[額外的資源](11-Bc-Hcm-additional-resources.md)。