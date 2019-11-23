---
title: 依據服務連接點設定自動託管快取探索
description: 本指南提供的指示說明如何在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ccc8b80537da0d0b689f6c508c75ef15a339c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406396"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>依據服務連接點設定自動託管快取探索

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

透過此程式，您可以使用群組原則，在執行下列 BranchCache\-支援的 Windows 作業系統的網域\-已加入電腦上，啟用及設定 BranchCache 託管快取模式。

- Windows 10 企業版
- Windows 10 教育版
- Windows 8.1 Enterprise
- Windows 8 企業版

> [!NOTE]  
> 若要設定執行 Windows Server 2008 R2 或 Windows 7 的已加入網域電腦，請參閱《 Windows Server 2008 R2 [BranchCache 部署指南》](https://technet.microsoft.com/library/ee649232.aspx)。

若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>若要使用群組原則來設定託管快取模式的用戶端

1. 在安裝 Active Directory Domain Services 伺服器角色的電腦上，開啟 [伺服器管理員]，選取本機伺服器，按一下 [**工具**]，然後按一下 [**群組原則管理**]。 [群組原則管理] 主控台隨即開啟。

2. 在 [群組原則管理主控台] 中，展開下列路徑 **：樹系：** *corp.contoso.com*、**網域**、 *corp.contoso.com*、**群組原則物件**，其中*corp.contoso.com*是您要設定的 BranchCache 用戶端電腦帳戶所在網域的名稱。

3. 在 [**群組原則物件**] 上按一下滑鼠\-右鍵，然後按一下 [**新增**]。 [**新增 GPO** ] 對話方塊隨即開啟。 在 [**名稱**] 中，輸入新群組原則物件 \(GPO\)的名稱。 例如，如果您想要將物件命名為 BranchCache 用戶端電腦，請輸入**Branchcache 用戶端電腦**。 按一下 **\[確定\]** 。

4. 在 [群組原則管理主控台] 中，確定已選取 [**群組原則物件**]，然後在詳細資料\-窗格中，按一下您剛才建立的 GPO。 例如，如果您將 GPO BranchCache 用戶端電腦命名為，請在 [ **Branchcache 用戶端電腦**] 上按一下滑鼠右鍵\-。 按一下 **\[編輯\]** 。 群組原則管理編輯器主控台隨即開啟。

5. 在群組原則管理編輯器主控台中，展開下列路徑： [**電腦**設定]、[**原則**]、[**系統管理範本：原則定義] \(ADMX 檔案\) 從本機電腦**、**網路**、 **BranchCache**中取出。

6. 按一下 [ **BranchCache**]，然後在詳細資料窗格中，按兩下 [**開啟 BranchCache**]\-。 [**開啟 BranchCache** ] 對話方塊隨即開啟。
  
7.  在 [**開啟 BranchCache** ] 對話方塊中，按一下 [**已啟用**]，然後按一下 **[確定]** 。

8. 在群組原則管理編輯器主控台中，確定仍已選取 [ **BranchCache** ]，然後在詳細資料窗格中，按兩下 [**依服務連接點啟用自動託管**快取探索]\-。 [原則設定] 對話方塊隨即開啟。

9. 在 [**依服務連接點啟用自動託管**快取探索] 對話方塊中，按一下 [**已啟用**]，然後按一下 **[確定]** 。

10. 若要讓用戶端電腦從 BranchCache 檔案伺服器\-內容伺服器下載並快取內容：在群組原則管理編輯器主控台中，確定仍已選取 [ **branchcache** ]，然後在詳細資料窗格中，\-按兩下 [網路檔案的**branchcache**]。 [**設定網路**檔案的 BranchCache] 對話方塊隨即開啟。 
11. 在 [**設定網路**檔案的 BranchCache] 對話方塊中，按一下 [**已啟用**]。 在 [**選項**] 中，輸入數值（以毫秒為單位），以取得最大來回行程網路延遲，然後按一下 **[確定]** 。
  
    > [!NOTE]
    > 根據預設，如果來回網路延遲超過80毫秒，用戶端電腦就會從檔案伺服器快取內容。
  
12. 若要設定每部用戶端電腦上為 BranchCache 快取配置的硬碟空間量：在群組原則管理編輯器主控台中，確定仍然選取 [ **BranchCache** ]，然後在詳細資料窗格中，\-按兩下 [**設定用於用戶端電腦快取的磁碟空間百分比**]。 [**設定用於用戶端電腦快取的磁碟空間百分比**] 對話方塊隨即開啟。 按一下 [**已啟用**]，然後在 [**選項**] 中輸入數值，代表 BranchCache 快取的每部用戶端電腦上所使用的硬碟空間百分比。 按一下 **\[確定\]** 。

13. 若要指定區段在用戶端電腦上的 BranchCache 資料快取中有效的預設存留期（以天為單位）：在群組原則管理編輯器主控台中，確定仍已選取 [ **branchcache** ]，然後在詳細資料窗格中\-按兩下 **[設定資料快取中的區段存留期**]。 [**設定資料快取中的區段存留期**] 對話方塊隨即開啟。 按一下 [**已啟用**]，然後在 [詳細資料] 窗格中輸入您偏好的天數。 按一下 **\[確定\]** 。

14. 針對您的部署，設定適用于用戶端電腦的其他 BranchCache 原則設定。

15. 執行命令**gpupdate/force**，或重新開機用戶端電腦，以更新分公司用戶端電腦上的群組原則。

您的 BranchCache 託管快取模式部署現在已完成。

如需本指南中之技術的詳細資訊，請參閱[其他資源](11-Bc-Hcm-additional-resources.md)。