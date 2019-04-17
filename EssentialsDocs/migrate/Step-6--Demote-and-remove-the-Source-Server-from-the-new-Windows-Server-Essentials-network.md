---
title: "步驟 6： 降級並移除新的 Windows Server Essentials 網路來源伺服器"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="step-6-demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network"></a>步驟 6： 降級並移除新的 Windows Server Essentials 網路來源伺服器

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

在您完成安裝 Windows Server Essentials 完成移轉之後，您必須執行下列工作：  
  
1.  [移除 Active Directory 憑證服務](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_ADCS)  
  
2.  [拔除直接連接到來源伺服器的印表機](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)  
  
3.  [降級來源伺服器](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)  
  
4.  [移除和重新安排來源伺服器](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)  
  
##  <a name="BKMK_ADCS"></a>移除 Active Directory 憑證服務  
 此程序會略有不同，如果您在單一伺服器上安裝多個 Active Directory 憑證 Services (AD CS) 角色服務。 若要解除安裝 AD CS 角色服務，並保留其他 AD CS 角色服務，您可以使用下列程序。  
  
 若要完成此程序，您必須使用相同的權限憑證授權單位已安裝的使用者來登入。 如果您的企業 CA 解除安裝，在企業系統管理員或相等成員資格是才能完成此程序最小值。  
  
#### <a name="to-remove-ad-cs"></a>若要移除 AD CS  
  
1.  網域系統管理員身分登入的來源伺服器。  
  
2.  按一下**[開始]**，按一下 [**系統管理工具]**，然後按一下 [**伺服器管理員**。  
  
3.  按一下**繼續**在**使用者 Account 控制項**對話方塊。  
  
4.  在**角色摘要**區段中，按**移除角色**。  
  
5.  在移除角色精靈中，按一下 [**下一步**。  
  
6.  清除**Active Directory 憑證服務**核取方塊，並再按**下**。  
  
7.  在**確認移除選項**頁面上，檢視資訊，然後按一下**移除**。  
  
    > [!NOTE]
    >  如果執行網際網路服務 (IIS)，以停止之前服務會提示您。 按一下**[確定]**。  
  
    > [!NOTE]
    >  首先，您可能必須移除**憑證授權單位 Web 註冊**，如果安裝它。  
  
8.  移除角色精靈完成時，重新開機才能完成解除安裝程序的伺服器。  
  
    > [!IMPORTANT]
    >  重新開機伺服器，即使就不會提示來執行動作。  
  
##  <a name="BKMK_PhysicallyDisconnect"></a>拔除直接連接到來源伺服器的印表機  
 您降級來源伺服器之前，拔任何直接連接到來源伺服器，並透過來源伺服器共用的印表機。 確保已直接連接到來源伺服器印表機不 Active Directory 物件。 印表機然後直接連接至目標伺服器，從 Windows Server Essentials 共用。  
  
##  <a name="BKMK_DemoteTheSourceServer"></a>降級來源伺服器  
 您將的網域成員伺服器角色來源伺服器角色 AD DS 網域控制站的降級之前，請確定 [群組原則設定的套用到所有 client 電腦，，如下所述。  
  
> [!IMPORTANT]
>  來源伺服器和目的地伺服器必須連接到網路時，群組原則變更更新 client 電腦上。  
  
#### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>若要對 client 電腦實施 「 群組原則 」 更新  
  
1.  以系統管理員身分登入 client 的電腦。  
  
2.  以系統管理員身分開放命令提示字元視窗。  
  
3.  在命令提示字元中，輸入**gpupdate /force**，然後按 ENTER 鍵。  
  
4.  此程序可能需要先登出並重新登入來完成。 按一下**[是]**來確認。  
  
 如果您從 Windows Server Essentials 或其舊版移轉，降級伺服器，查看[移除 Active Directory Domain Services](https://technet.microsoft.com/library/hh472163.aspx)。 您的工作群組成員加入來源伺服器，並將它拔除從網路後，您必須將它移除 AD DS 目的伺服器上。  
  
 如果您從 Windows Server Essentials 移轉，使用伺服器管理員移除，藉此降級使用下列程序來源伺服器上的網域控制站 Active Directory Domain Services 的角色：  
  
#### <a name="to-remove-the-source-server-from-active-directory"></a>若要移除 Active Directory 來源伺服器  
  
1.  在目的伺服器，請打開**Active Directory 使用者和電腦**。  
  
2.  在**Active Directory 使用者和電腦**瀏覽窗格中，依序展開網域名稱和**電腦**。  
  
3.  如果來源伺服器仍然存在伺服器清單中，以滑鼠右鍵按一下 [來源伺服器名稱，請按一下**Delete**，然後按**是**。  
  
4.  確認來源伺服器未列出，和 [關閉**Active Directory 使用者和電腦**。  
  
##  <a name="BKMK_RemoveTheSourceServer"></a>移除和重新安排來源伺服器  
 關閉來源伺服器，並將它拔除從網路。 我們建議您無法重新格式化來源伺服器至少一個星期了，以確保，所需的所有資料都移轉到目的伺服器。 確認您的所有資料都移轉之後，您可以重新安裝此伺服器網路上的次要伺服器的其他工作，如果需要的話。  
  
> [!NOTE]
>  您降級並移除來源伺服器之後，請重新開機目的伺服器。  
  
 您降級來源伺服器之後，它不健全狀態。 如果您想要重新安排來源伺服器，最簡單的方式是重新設定，安裝伺服器作業系統，然後再設定，以便使用其他伺服器以搭配。  
  
## <a name="next-steps"></a>後續步驟  
 您有降級並移除新的 Windows Server Essentials 網路來源伺服器。 立即移至[執行 「 步驟 7： 執行的 Windows Server Essentials 移轉後的移轉作業](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md)。  
  

若要檢視所有的步驟，請查看[Windows Server essentials 移轉](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

