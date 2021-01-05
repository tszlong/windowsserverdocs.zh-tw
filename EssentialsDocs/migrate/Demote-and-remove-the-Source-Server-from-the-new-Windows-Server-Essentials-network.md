---
title: 從新的 Windows Server Essentials 的伺服器降級和移除來源伺服器
description: 瞭解如何從新的 Windows Server Essentials 網路降級和移除來源伺服器。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: d9f18b29-8e03-439e-bdf0-1dac5e4f70c5
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 792aa83ed86c620921f243eba856f76258ab638f
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811035"
---
# <a name="demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network1"></a>從新的 Windows Server Essentials 的伺服器降級和移除來源伺服器

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

在您完成安裝 Windows Server Essentials 並在 [遷移嚮導] 中完成工作之後，您必須執行下列工作：


1.  [卸載 Exchange Server 2003](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003)。

2.  [中斷直接連線到來源伺服器的印表機](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)。

3.  [將來源伺服器降級](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)。

4.  [將 DHCP 伺服器角色從來源伺服器移到路由器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole)。

5.  [移除和重新規劃來源伺服器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)。


###  <a name="uninstall-exchange-server-2003"></a><a name="BKMK_UninstallExchangeServer2003"></a> 卸載 Exchange Server 2003

> [!IMPORTANT]
>  如果您新增使用者帳戶的時間是在信箱移到目的地伺服器之後和從來源伺服器解除安裝 Exchange Server 2003 之前，則信箱會新增到來源伺服器上。 這是原廠設定。 您必須將這段時間內新增的所有使用者帳戶的信箱移到目的地伺服器。 在卸載 Exchange Server 2003 之前，請重複「移動 Exchange Server 信箱及設定以進行 Windows Server Essentials 遷移」中的指示。

 在您將來源伺服器降級之前，您必須從來源伺服器解除安裝 Exchange Server 2003。 這樣會移除在 Active Directory 網域服務 (AD DS) 中對來源伺服器上 Exchange Server 的所有參照。 您必須備妥您的 Windows Small Business Server 2003 媒體才能移除 Exchange Server 2003。

##### <a name="to-uninstall-exchange-server-2003-from-the-source-server"></a>從來源伺服器解除安裝 Exchange Server 2003

1. 以系統管理員身分登入來源伺服器。

2. 按一下 [開始]、[控制台]，然後按一下 [新增或移除程式]。

3. 在程式清單中，選取 [Windows Small Business Server 2003]，然後按一下 [變更/移除]。

4. 在 [安裝精靈] 中持續按 [下一步]，直到 [選擇元件] 頁面顯示為止。

5. 在 [選擇元件] 頁面中，展開 [Exchange Server]，然後選擇 [移除]。

   > [!NOTE]
   >
   >  Exchange Server 會檢查伺服器上是否已沒有信箱或公用資料夾。 如果有遺留的資料，當您按一下 [移除] 時會出現錯誤訊息。 若要避免這個問題，請確定您已完成將 [SBS 2003 設定和資料移至目的地伺服器](./move-windows-sbs-2003-to-the-destination-server-for-migration.md)主題中的所有程式。


6. 按一下 [下一步] 。

7. 出現提示時，插入 Windows Small Business Server 2003 CD#3，並依照畫面上的指示進行。

###  <a name="disconnect-printers-that-are-directly-connected-to-the-source-server"></a><a name="BKMK_PhysicallyDisconnect"></a> 中斷直接連線到來源伺服器的印表機
 在降級來源伺服器之前，請中斷任何直接連線到來源伺服器，並透過來源伺服器共用的印表機。 請確定沒有保留任何直接連線到來源伺服器的印表機的 Active Directory 物件。 然後，印表機可以直接連接到目的地伺服器，並從 Windows Server Essentials 共用。

###  <a name="demote-the-source-server"></a><a name="BKMK_DemoteTheSourceServer"></a> 降級來源伺服器
 在將來源伺服器從 AD DS 網域控制站的角色降級到網域成員伺服器角色之前，請確定群組原則設定會套用到所有的用戶端電腦，如下列程序所述。

> [!IMPORTANT]
>  在更新用戶端電腦上的群組原則時，來源伺服器與目的地伺服器必須連線到網路。

##### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>強制更新用戶端電腦上的群組原則

1.  以系統管理員身分登入用戶端電腦。

2.  以系統管理員身分開啟 [命令提示字元] 視窗。

3.  在命令提示字元中，輸入 **gpupdate /force**，然後按 ENTER。

4.  可能需要您登出再重新登入，以完成處理程序。 按一下 [是]  確認。

##### <a name="to-demote-the-source-server"></a>將來源伺服器降級

1. 在來源伺服器上，按一下 [開始]、[執行]，輸入 **dcpromo**，然後按一下 [確定]。

2. 按兩次 [下一步]。

   > [!NOTE]
   >  請勿選取 [這台伺服器是網域中的最後一個網域控制站]。

3. 在伺服器上，輸入新的系統管理員帳戶的密碼，然後按一下 [下一步]。

4. 在 [摘要] 對話方塊中，會通知您將從電腦中移除 AD DS，且伺服器會成為網域成員。 按 [下一步]  。

5. 按一下 [完成] 。 來源伺服器會重新啟動。

6. 在來源伺服器重新啟動之後，將來源伺服器新增為工作群組的成員，再中斷來源伺服器的網路連線。

   在將來源伺服器新增為工作群組成員，並中斷其網路連線之後，您必須將它從目的地伺服器上的 AD DS 中移除。

##### <a name="to-remove-the-source-server-from-active-directory"></a>從 Active Directory 中移除來源伺服器

1.  在目的地伺服器上，開啟 [Active Directory 使用者和電腦]。

2.  在 [Active Directory 使用者和電腦] 瀏覽窗格中，展開網域名稱，然後再展開 [電腦]。

3.  如果來源伺服器名稱存在於伺服器清單中，請以滑鼠右鍵按一下，按一下 [刪除]，然後按一下 [是]。

4.  請確認沒有列出來源伺服器，並關閉 [Active Directory 使用者和電腦]。

###  <a name="move-the-dhcp-server-role-from-the-source-server-to-the-router"></a><a name="BKMK_MoveTheDHCPRole"></a> 將 DHCP 伺服器角色從來源伺服器移至路由器

> [!NOTE]
>
>  如果您在開始移轉程序之前已執行過這項工作，請繼續[移除來源伺服器和變更來源伺服器用途](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)一節。


 如果您的來源伺服器正在執行 DHCP 角色，請執行下列步驟，將 DHCP 角色移至路由器。

##### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>將 DHCP 角色從來源伺服器移動到路由器

1.  如下所示關閉來源伺服器上的 DHCP 服務：

    1.  在來源伺服器上，依序按一下 [開始]、[系統管理工具]，然後按一下 [服務]。

    2.  在目前執行的服務清單中，以滑鼠右鍵按一下 [Windows Server]，然後按一下 [內容]。

    3.  於 [啟動類型] 選取 [停用]。

    4.  停止服務。

2.  開啟您的路由器上的 DHCP 角色

    1.  依照路由器的文件指示，開啟路由器上的 DHCP 角色。

    2.  若要確保來源伺服器所發出的 IP 位址維持不變，請依照路由器文件中的指示，將路由器上的 DHCP 範圍設定為與來源伺服器上的 DHCP 範圍相同。

    > [!IMPORTANT]
    >  如果您沒有為目的地伺服器的路由器設定靜態 IP 或 DHCP 保留區，且 DHCP 範圍與來源伺服器的 DHCP 範圍不相同，路由器可能為目的地伺服器發出新的 IP 位址。 如果發生這種情況，請重設路由器的連接埠轉送規則，轉送至目的地伺服器的新 IP 位址。

###  <a name="remove-and-repurpose-the-source-server"></a><a name="BKMK_RemoveTheSourceServer"></a> 移除和重新規劃來源伺服器
 關閉來源伺服器，並中斷其網路連線。 建議您至少一週不要重新格式化來源伺服器，以確保所有必要的資料都移轉到目的地伺服器。 確認已移轉所有資料後，如有必要，您可以在網路上重新安裝這部伺服器，作為其他工作的次要伺服器。

> [!NOTE]
>  在您降級並移除來源伺服器後，請重新啟動目的地伺服器。

 降級來源伺服器之後，它處於不健全的狀態。 如果您想要重新規劃來源伺服器，最簡單的方式是將它重新格式化、安裝伺服器作業系統，然後將它設定為其他伺服器使用。