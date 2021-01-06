---
title: 啟用非網域成員檔案伺服器的雜湊發行
description: 瞭解如何在已安裝檔案服務伺服器角色的 BranchCache for Network Files 角色服務的檔案伺服器上，使用本機電腦群組原則設定 BranchCache 的雜湊發行。
manager: brianlic
ms.topic: how-to
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: 9da8c5677f3dd137fb682a17adc55da29f973f82
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948434"
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>啟用非網域成員檔案伺服器的雜湊發行

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用這個程式，針對已安裝檔案服務伺服器角色的 [網路檔案的 **branchcache** ] 角色服務，在執行 Windows server 2016 的檔案伺服器上，使用本機電腦群組原則設定 branchcache 的雜湊發行。

此程式適用于非網域成員檔案伺服器。 如果您在網域成員檔案伺服器上執行此程式，而且也使用網域群組原則設定 BranchCache，網域群組原則設定會覆寫本機群組原則設定。

若要執行此程式，至少需要 **Administrators** 的成員資格或同等許可權。

> [!NOTE]
> 如果您有一或多個網域成員檔案伺服器，您可以在 Active Directory Domain Services 中將它們新增至組織單位， (OU) ，然後使用群組原則為所有檔案伺服器設定雜湊發行，而不是個別設定每個檔案伺服器。 如需詳細資訊，請參閱 [啟用網域成員檔案伺服器的雜湊發行](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。

### <a name="to-enable-hash-publication-for-one-file-server"></a>啟用單一檔案伺服器的雜湊發行

1.  開啟 Windows PowerShell，然後輸入 **mmc**，再按 ENTER 鍵。 此時會開啟 Microsoft Management Console (MMC)。

2.  在 MMC 的 **[檔案]** 功能表上，按一下 **[新增/移除嵌入式管理單元]**。 [ **新增或移除嵌入式管理單元** ] 對話方塊隨即開啟。

3.  在 [ **新增或移除嵌入式管理單元**] 的 [可用的嵌入式 **管理** 單元] 中，按兩下 [ **群組原則物件編輯器**]。 群組原則 Wizard 會開啟，並選取 [本機電腦] 物件。 按一下 [完成]，然後按一下 [確定]。

4.  在本機群組原則編輯器 MMC 中，展開下列路徑： [本機 **電腦原則**]、[ **電腦** 設定]、[ **系統管理範本**]、[ **網路**]、[ **Lanman 伺服器**]。 按一下 [ **Lanman 伺服器**]。

5.  在詳細資料窗格中，按兩下 [ **BranchCache 的雜湊發行**]。 [ **BranchCache 的雜湊發行** ] 對話方塊隨即開啟。

6.  在 [ **BranchCache 的雜湊發行** ] 對話方塊中，按一下 [ **已啟用**]。

7.  在 [ **選項**] 中，按一下 [ **允許所有共用資料夾的雜湊發行**]，然後按一下下列其中一項：

    1.  若要針對這部電腦上的所有共用資料夾啟用雜湊發行，請按一下 [ **允許所有共用資料夾的雜湊發行**]。

    2.  若只要針對啟用 BranchCache 的共用資料夾啟用 [雜湊發行]，請按一下 [ **只允許啟用 branchcache 的共用資料夾的雜湊發行**]。

    3.  若要對電腦上的所有共用資料夾不允許雜湊發行，即使已在檔案共用上啟用 BranchCache，請按一下 [不 **允許在所有共用資料夾上進行雜湊發行**]。

8.  按一下 [確定]  。



