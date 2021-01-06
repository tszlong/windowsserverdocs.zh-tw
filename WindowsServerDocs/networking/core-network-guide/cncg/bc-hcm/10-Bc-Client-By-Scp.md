---
title: 依據服務連接點設定自動託管快取探索
description: 本指南提供在執行 Windows Server 2016 和 Windows 10 的電腦上，以託管快取模式部署 BranchCache 的指示。
manager: brianlic
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 05866d3984dcc8185aca6a40e9402455fb703095
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947894"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>依據服務連接點設定自動託管快取探索

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用此程式時，您可以使用群組原則，在已加入網域的電腦上啟用及設定 BranchCache 託管快取模式，而 \- 這些電腦執行下列支援 branchcache 的 \- Windows 作業系統。

- Windows 10 Enterprise
- Windows 10 Education
- Windows 8.1 Enterprise
- Windows 8 企業版

> [!NOTE]
> 若要設定執行 Windows Server 2008 R2 或 Windows 7 的已加入網域電腦，請參閱《 Windows Server 2008 R2 [BranchCache 部署指南》](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee649232(v=ws.10))。

若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>使用群組原則設定託管快取模式的用戶端

1. 在安裝 Active Directory Domain Services server 角色的電腦上，開啟伺服器管理員，選取本機伺服器，按一下 [ **工具**]，然後按一下 [ **群組原則管理**]。 此時會開啟群組原則管理主控台。

2. 在群組原則管理主控台中，展開下列路徑： [ **樹系：** *corp.contoso.com*]、[ **網域**]、[ *corp.contoso.com*]、[ **群組原則物件**]，其中 *corp.contoso.com* 是您要設定的 BranchCache 用戶端電腦帳戶所在網域的名稱。

3. 以滑鼠右鍵 \- 按一下 **群組原則物件**]，然後按一下 [ **新增**]。 [ **新增 GPO** ] 對話方塊隨即開啟。 在 [ **名稱**] 中，輸入新群組原則物件 GPO 的名稱 \( \) 。 例如，如果您想要命名物件 BranchCache 用戶端電腦，請輸入 **Branchcache 用戶端電腦**。 按一下 [確定]。

4. 在群組原則管理主控台中，確定已選取 [ **群組原則物件** ]，然後在 [詳細資料] 窗格中，以滑鼠右鍵 \- 按一下您剛才建立的 GPO。 例如，如果您將 GPO BranchCache 用戶端電腦命名為，請以滑鼠右鍵 \- 按一下 [ **Branchcache 用戶端電腦**]。 按一下 **[編輯]** 。 群組原則管理編輯器主控台隨即開啟。

5. 在群組原則管理編輯器主控台中，展開下列路徑： [**電腦** 設定]、[**原則**]、 **[系統管理範本： \( \) 從本機電腦、網路、BranchCache 取出的原則定義 ADMX** 檔案。  

6. 按一下 [ **branchcache**]，然後在詳細資料窗格中，按兩下 [ \- **開啟 branchcache**]。 [ **開啟 BranchCache** ] 對話方塊隨即開啟。

7.  在 [ **開啟 BranchCache** ] 對話方塊中，按一下 [ **已啟用**]，然後按一下 **[確定]**。

8. 在群組原則管理編輯器主控台中，確定仍選取 [ **BranchCache** ]，然後在 [詳細資料] 窗格中，按兩下 [ \- **依服務連接點啟用自動託管** 快取探索]。 [原則設定] 對話方塊隨即開啟。

9. 在 [ **依服務連接點啟用自動託管** 快取探索] 對話方塊中，按一下 [ **已啟用**]，然後按一下 **[確定]**。

10. 若要讓用戶端電腦從 BranchCache 檔案伺服器型內容伺服器下載及快取內容 \- ：在群組原則管理編輯器主控台中，確定仍選取 [ **branchcache** ]，然後在 [詳細資料] 窗格中，按兩下 [ \- 網路檔案 **的 branchcache**]。 [ **設定 BranchCache for network files** ] 對話方塊隨即開啟。
11. 在 [ **設定 BranchCache for network files** ] 對話方塊中，按一下 [ **已啟用**]。 在 [ **選項**] 中，輸入以毫秒為單位的數值（以毫秒為單位），表示最大的往返網路延遲，然後按一下 **[確定]**。

    > [!NOTE]
    > 根據預設，如果往返網路延遲超過80毫秒，用戶端電腦就會從檔案伺服器快取內容。

12. 若要針對 BranchCache 快取設定每部用戶端電腦上配置的硬碟空間量：在群組原則管理編輯器主控台中，確定仍選取 [ **BranchCache** ]，然後在詳細資料窗格中，按兩下 [ \- **設定用於用戶端電腦快取的磁碟空間百分比**]。 [ **設定用於用戶端電腦快取的磁碟空間百分比** ] 對話方塊隨即開啟。 按一下 [ **已啟用**]，然後在 [ **選項** ] 中輸入數值，代表 BranchCache 快取在每部用戶端電腦上使用的硬碟空間百分比。 按一下 [確定]。

13. 若要指定在用戶端電腦的 BranchCache 資料快取中有效區段的預設存留期（以天為單位）：在群組原則管理編輯器主控台中，確定仍選取 [ **BranchCache** ]，然後在 [詳細資料] 窗格中，按兩下 \- **[設定資料快取中區段的存留期**]。 [資料快取] 對話方塊中的 [ **設定區段存留期** ] 對話方塊隨即開啟。 按一下 [ **已啟用**]，然後在 [詳細資料] 窗格中輸入您想要的天數。 按一下 [確定]。

14. 針對您的部署適當地設定用戶端電腦的其他 BranchCache 原則設定。

15. 執行命令 **gpupdate/force** 或重新開機用戶端電腦，以重新整理分公司用戶端電腦上的群組原則。

您的 BranchCache 託管快取模式部署現在已完成。

如需本指南中技術的詳細資訊，請參閱 [其他資源](11-Bc-Hcm-additional-resources.md)。