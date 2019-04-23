---
title: 步驟 6：從新的 Windows Server Essentials 網路降級和移除來源伺服器
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 86244c66-2c5e-488d-adb8-112e1ca3e2e1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 24a1f2da2333c7e6854e9efd9d996391d0fcb3b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872359"
---
# <a name="step-6-demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network"></a>步驟 6：從新的 Windows Server Essentials 網路降級和移除來源伺服器

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

完成安裝 Windows Server Essentials，並完成移轉之後，您必須執行下列工作：  
  
1.  [移除 Active Directory 憑證服務](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_ADCS)  
  
2.  [中斷直接連線到來源伺服器的印表機](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)  
  
3.  [降級來源伺服器](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)  
  
4.  [移除和重新規劃來源伺服器](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)  
  
##  <a name="BKMK_ADCS"></a> 移除 Active Directory 憑證服務  
 如果您在單一伺服器上安裝多個 Active Directory 憑證服務 (AD CS) 角色服務，則程序會有些許不同。 您可以使用下列程序解除安裝 AD CS 角色服務，但保留其他的 AD CS 角色服務。  
  
 若要完成此程序，您必須使用相同於安裝憑證授權單位 (CA) 之使用者身分的權限來登入。 如果您正在解除安裝企業 CA，至少需要 Enterprise Admins 的成員資格或同等權限，才能完成此程序。  
  
#### <a name="to-remove-ad-cs"></a>移除 AD CS  
  
1.  以網域系統管理員身分登入來源伺服器。  
  
2.  依序按一下 [開始] 、[系統管理工具] ，然後按一下 [伺服器管理員] 。  
  
3.  按一下 **[使用者帳戶控制]** 對話方塊的 **[繼續]** 。  
  
4.  在 [角色摘要] 區段中，按一下 [移除角色]。  
  
5.  在 [移除角色精靈] 中，按一下 [下一步]。  
  
6.  清除 [Active Directory 憑證服務] 核取方塊，然後按一下 [下一步]。  
  
7.  檢視 [確認移除選項] 頁面上的資訊，然後按一下 [移除]。  
  
    > [!NOTE]
    >  如果網際網路資訊服務 (IIS) 正在執行，會提示您停止服務才能繼續。 按一下 [確定] 。  
  
    > [!NOTE]
    >  首先，您可能需要移除 [憑證授權單位網頁註冊] (如果已安裝)。  
  
8.  當 [移除角色精靈] 完成時，重新啟動伺服器以完成解除安裝程序。  
  
    > [!IMPORTANT]
    >  即使沒有提示您執行這項操作，也請重新啟動伺服器。  
  
##  <a name="BKMK_PhysicallyDisconnect"></a> 中斷直接連線到來源伺服器的印表機  
 在降級來源伺服器之前，請中斷任何直接連線到來源伺服器，並透過來源伺服器共用的印表機。 請確定沒有保留任何直接連線到來源伺服器的印表機的 Active Directory 物件。 印表機可以是直接連接到目的地伺服器並共用從 Windows Server Essentials。  
  
##  <a name="BKMK_DemoteTheSourceServer"></a> 降級來源伺服器  
 在將來源伺服器從 AD DS 網域控制站的角色降級到網域成員伺服器角色之前，請確定群組原則設定會套用到所有的用戶端電腦，如下列程序所述。  
  
> [!IMPORTANT]
>  在更新用戶端電腦上的群組原則時，來源伺服器與目的地伺服器必須連線到網路。  
  
#### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>強制更新用戶端電腦上的群組原則  
  
1.  以系統管理員身分登入用戶端電腦。  
  
2.  開啟命令提示字元視窗，以系統管理員身分執行。  
  
3.  在命令提示字元中，輸入 **gpupdate /force**，然後按 ENTER。  
  
4.  可能需要您登出再重新登入，以完成處理程序。 按一下 [是]  以確認。  
  
 如果您從 Windows Server Essentials 或其先前版本移轉，若要降級伺服器，請參閱[移除 Active Directory 網域服務](https://technet.microsoft.com/library/hh472163.aspx)。 在將來源伺服器新增為工作群組成員，並中斷其網路連線之後，您必須將它從目的地伺服器上的 AD DS 中移除。  
  
 如果您從 Windows Server Essentials 移轉，請使用伺服器管理員移除 Active Directory 網域服務角色，藉此降級網域控制站，使用下列程序在來源伺服器上：  
  
#### <a name="to-remove-the-source-server-from-active-directory"></a>從 Active Directory 中移除來源伺服器  
  
1.  在目的地伺服器上，開啟 [Active Directory 使用者和電腦]。  
  
2.  在 [Active Directory 使用者和電腦]  瀏覽窗格中，展開網域名稱，然後再展開 [電腦] 。  
  
3.  如果來源伺服器仍然在伺服器清單中，以滑鼠右鍵按一下來源伺服器名稱，按一下 [刪除]，然後按一下 [是]。  
  
4.  請確認沒有列出來源伺服器，並關閉 [Active Directory 使用者和電腦] 。  
  
##  <a name="BKMK_RemoveTheSourceServer"></a> 移除和重新規劃來源伺服器  
 關閉來源伺服器，並中斷其網路連線。 建議您至少一週不要重新格式化來源伺服器，以確保所有必要的資料都移轉到目的地伺服器。 確認已移轉所有資料後，如有必要，您可以在網路上重新安裝這部伺服器，作為其他工作的次要伺服器。  
  
> [!NOTE]
>  在您降級並移除來源伺服器後，請重新啟動目的地伺服器。  
  
 降級來源伺服器之後，它處於不健全的狀態。 如果您想要重新規劃來源伺服器，最簡單的方式是將它重新格式化、安裝伺服器作業系統，然後將它設定為其他伺服器使用。  
  
## <a name="next-steps"></a>後續步驟  
 您已經降級並從新的 Windows Server Essentials 網路中移除來源伺服器。 現在請移至[步驟 7:執行 Windows Server Essentials 移轉的移轉後工作](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md)。  
  

若要檢視所有步驟，請參閱[移轉至 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

