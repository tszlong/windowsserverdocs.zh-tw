---
title: 將現有檔案伺服器設定為內容伺服器
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ddb6a9d8ad14146d6f4aca27171649fde3b227eb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971915"
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>將現有檔案伺服器設定為內容伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，在執行 Windows Server 2016 的電腦上安裝檔案服務伺服器角色的 [網路檔案的**BranchCache** ] 角色服務。

> [!IMPORTANT]
> 如果尚未安裝檔案服務伺服器角色，請不要遵循此程式。 相反地，請參閱[安裝新的檔案伺服器做為內容伺服器](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md)。

若要執行此程式，至少需要**Administrators**的成員資格或同等許可權。

> [!NOTE]
> 若要使用 Windows PowerShell 執行此程式，請以系統管理員身分執行 Windows PowerShell，在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER。
>
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`
>
> 若要安裝重復資料刪除角色服務，請輸入下列命令，然後按 ENTER。
>
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`

### <a name="to-install-the-branchcache-for-network-files-role-service"></a>安裝網路檔案的 BranchCache 角色服務

1.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能精靈] 隨即開啟。 按 [下一步]  。

2.  在 [**選取安裝類型**] 中，確定已選取 [以**角色為基礎] 或 [功能型安裝**]，然後按 **[下一步]**。

3.  在 [**選取目的地伺服器**] 中，確定已選取正確的伺服器，然後按 **[下一步]**。

4.  在 [**選取伺服器角色**] 的 [**角色**] 中，請注意檔案**和存放服務**角色已安裝;按一下角色名稱左側的箭號以展開 [角色服務] 的選取範圍，然後按一下 [檔案**和 ISCSI 服務**] 左邊的箭號。

5.  選取 [網路檔案的**BranchCache**] 核取方塊。

    > [!TIP]
    > 如果您尚未這麼做，建議您同時選取**重復資料刪除**的核取方塊。

    按 [下一步]  。

6.  在 [**選取功能**] 中，按 **[下一步]**。

7.  在 [**確認安裝選項**] 中，檢查您的選擇，然後按一下 [**安裝**]。 安裝期間會顯示 [**安裝進度**] 窗格。 安裝完成時，按一下 [**關閉**]。



