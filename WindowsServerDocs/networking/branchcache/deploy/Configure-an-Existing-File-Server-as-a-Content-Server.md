---
title: 將現有檔案伺服器設定為內容伺服器
description: 瞭解如何在執行 Windows Server 2016 的電腦上安裝檔案服務伺服器角色的 [網路檔案的 BranchCache] 角色服務。
manager: brianlic
ms.topic: how-to
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: b00584280f2bf18952b88f88e851cddca8d842f1
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948504"
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>將現有檔案伺服器設定為內容伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，在執行 Windows Server 2016 的電腦上安裝檔案服務伺服器角色的 [網路檔案的 **BranchCache** ] 角色服務。

> [!IMPORTANT]
> 如果尚未安裝檔案服務伺服器角色，請勿遵循此程式。 請改 [為參閱將新的檔案伺服器安裝為內容伺服器](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md)。

若要執行此程式，至少需要 **Administrators** 的成員資格或同等許可權。

> [!NOTE]
> 若要使用 Windows PowerShell 來執行此程式，請以系統管理員身分執行 Windows PowerShell，在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER 鍵。
>
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`
>
> 若要安裝重復資料刪除角色服務，請輸入下列命令，然後按 ENTER。
>
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`

### <a name="to-install-the-branchcache-for-network-files-role-service"></a>安裝網路檔案的 BranchCache 角色服務

1.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能精靈] 隨即開啟。 按一下 [下一步] 。

2.  在 [ **選取安裝類型**] 中，確定已選取 [以 **角色為基礎或以功能為基礎的安裝** ]，然後按 **[下一步]**。

3.  在 [ **選取目的地伺服器**] 中，確定已選取正確的伺服器，然後按 **[下一步]**。

4.  在 [ **選取伺服器角色**] 的 [ **角色**] 中，請注意已安裝檔案 **和存放服務** 角色;按一下角色名稱左邊的箭號，以展開選取的角色服務，然後按一下 [檔案 **和 ISCSI 服務**] 左邊的箭號。

5.  選取 [網路檔案的 **BranchCache**] 核取方塊。

    > [!TIP]
    > 如果您尚未這麼做，建議您也選取 [ **重復資料刪除**] 核取方塊。

    按一下 [下一步] 。

6.  在 [ **選取功能**] 中，按 **[下一步]**。

7.  在 [ **確認安裝選項**] 中，檢查您的選項，然後按一下 [ **安裝**]。 安裝期間會顯示 [ **安裝進度** ] 窗格。 安裝完成時，按一下 [ **關閉**]。



