---
title: "降級並移除新的 Windows Server Essentials network1 來源伺服器"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9f18b29-8e03-439e-bdf0-1dac5e4f70c5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1545189732194ad5c0aba401f834b0102799e016
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network1"></a>降級並移除新的 Windows Server Essentials network1 來源伺服器

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您完成安裝 Windows Server Essentials 移轉精靈中的工作完成之後，您必須執行下列工作：  
  

1.  [解除安裝 Exchange Server 2003](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003)。  
  
2.  [拔除直接連接到來源伺服器的印表機](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)。  
  
3.  [降級來源伺服器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)。  
  
4.  [從來源伺服器 DHCP 伺服器角色前往路由器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole)。  
  
5.  [移除和重新安排來源伺服器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)。  

1.  [解除安裝 Exchange Server 2003](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003)。  
  
2.  [拔除直接連接到來源伺服器的印表機](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)。  
  
3.  [降級來源伺服器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)。  
  
4.  [從來源伺服器 DHCP 伺服器角色前往路由器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole)。  
  
5.  [移除和重新安排來源伺服器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)。  

  
###  <a name="BKMK_UninstallExchangeServer2003"></a>解除安裝 Exchange Server 2003  
  
> [!IMPORTANT]
>  如果您移動到目的伺服器和解除安裝的來源伺服器 Exchange Server 2003 之前信箱後，您就會加入帳號，信箱加入來源伺服器上。 這是設計。 您必須信箱移到所有使用者帳號，會在這段期間目的伺服器。 Exchange Server 2003 解除安裝之前，Windows Server Essentials 移轉重複移動 Exchange Server 信箱和設定] 中的指示操作。  
  
 您將其之前，您必須解除 Exchange Server 2003 從來源伺服器。 這樣會移除所有的參考中換貨伺服器來源伺服器上的 Active Directory Domain Services (AD DS)。 您必須移除 Exchange Server 2003 Windows 小型企業 Server 2003 媒體。  
  
##### <a name="to-uninstall-exchange-server-2003-from-the-source-server"></a>若要解除安裝 Exchange Server 2003，從來源伺服器  
  
1.  以系統管理員身分登入的來源伺服器  
  
2.  按一下**[開始]**，按一下 [ **[控制台]**，然後按一下 [**新增或移除程式**。  
  
3.  在 [程式] 清單，選取 [ **Windows 小型企業 Server 2003**，然後按一下 [**變更或移除**。  
  
4.  在安裝精靈中，按一下**下一步**直到**元件選取**頁面隨即顯示。  
  
5.  在元件選取項目頁面中，展開**Exchange Server**，然後選擇 [**移除**。  
  
    > [!NOTE]

    >  Exchange Server 會檢查以確認有任何信箱或公開伺服器上的資料夾。 如果任何資料會保留，出現錯誤訊息，當您按一下**移除**。 若要避免這個問題，請確定您已完成此主題中的程序的所有[移動 SBS 2003 設定和資料目的伺服器以](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  

    >  Exchange Server 會檢查以確認有任何信箱或公開伺服器上的資料夾。 如果任何資料會保留，出現錯誤訊息，當您按一下**移除**。 若要避免這個問題，請確定您已完成此主題中的程序的所有[移動 SBS 2003 設定和資料目的伺服器以](../migrate/Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md)。  

  
6.  按一下**下一步**。  
  
7.  出現提示時，插入 Windows 小型企業 Server 2003 CD #3，並依照畫面上的指示。  
  
###  <a name="BKMK_PhysicallyDisconnect"></a>拔除直接連接到來源伺服器的印表機  
 您降級來源伺服器之前，拔任何直接連接到來源伺服器，並透過來源伺服器共用的印表機。 確保已直接連接到來源伺服器印表機不 Active Directory 物件。 印表機然後直接連接至目標伺服器，從 Windows Server Essentials 共用。  
  
###  <a name="BKMK_DemoteTheSourceServer"></a>降級來源伺服器  
 您將的網域成員伺服器角色來源伺服器角色 AD DS 網域控制站的降級之前，請確定 [群組原則設定的套用到所有 client 電腦，，如下所述。  
  
> [!IMPORTANT]
>  來源伺服器和目的地伺服器必須連接到網路時，群組原則變更更新 client 電腦上。  
  
##### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>若要對 client 電腦實施 「 群組原則 」 更新  
  
1.  以系統管理員身分登入 client 的電腦。  
  
2.  以系統管理員身分開放命令提示字元視窗。  
  
3.  在命令提示字元中，輸入**gpupdate /force**，然後按 ENTER 鍵。  
  
4.  此程序可能需要先登出並重新登入來完成。 按一下**[是]**來確認。  
  
##### <a name="to-demote-the-source-server"></a>若要降級來源伺服器  
  
1.  在來源伺服器上，按一下 [ **[開始]**，按一下 [**執行**，輸入**帶領**，，然後按一下**[確定]**。  
  
2.  按一下**下一步**兩次。  
  
    > [!NOTE]
    >  不要選取 [**這個伺服器會網域中的最後一個網域控制站**。  
  
3.  在伺服器上，輸入新的系統管理員 account 的密碼，然後按一下**下一步**。  
  
4.  在**摘要**對話方塊中，AD DS，將會從電腦移除和伺服器會網域通知您。 按一下**下一步**。  
  
5.  按一下**完成**。 來源伺服器重新開機。  
  
6.  來源伺服器重新開機之後，新增來源伺服器工作群組成員之前中斷網路。  
  
 您的工作群組成員加入來源伺服器，並將它拔除從網路後，您必須將它移除 AD DS 目的伺服器上。  
  
##### <a name="to-remove-the-source-server-from-active-directory"></a>若要移除 Active Directory 來源伺服器  
  
1.  在目的伺服器，請打開**Active Directory 使用者和電腦**。  
  
2.  在**Active Directory 使用者和電腦**瀏覽窗格中，依序展開網域名稱和**電腦**。  
  
3.  以滑鼠右鍵按一下來源伺服器名稱，如果仍然存在伺服器清單中按一下**Delete**，然後按一下 [**是**。  
  
4.  確認來源伺服器未列出，和 [關閉**Active Directory 使用者和電腦**。  
  
###  <a name="BKMK_MoveTheDHCPRole"></a>從來源伺服器前往路由器 DHCP 伺服器角色  
  
> [!NOTE]

>  如果您執行這項工作移轉程序開始之前，請繼續區段[請移除並重新安排來源伺服器](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)。  

>  如果您執行這項工作移轉程序開始之前，請繼續區段[請移除並重新安排來源伺服器](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)。  

  
 如果您的來源伺服器 DHCP 角色執行，執行下列步驟移動 DHCP 角色路由器。  
  
##### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>前往 DHCP 角色從來源伺服器路由器  
  
1.  要關閉 DHCP 伺服器上的服務來源，方式如下：  
  
    1.  來源在伺服器上，按一下 [ **[開始]**，按一下**系統管理工具]**，然後按一下 [**服務**。  
  
    2.  在清單中目前執行的服務，以滑鼠右鍵按一下**Windows Server**，然後按**屬性**。  
  
    3.  適用於**開始輸入]**、**停用**。  
  
    4.  停止服務。  
  
2.  將您的路由器上 DHCP 角色  
  
    1.  請依照您的路由器的文件中的指示，將在路由器上 DHCP 角色。  
  
    2.  若要確保發出來源伺服器的 IP 位址維持不變，依照您的路由器的文件中的指示來設定 DHCP 範圍來源伺服器上的 DHCP 範圍相同路由器上。  
  
    > [!IMPORTANT]
    >  如果您有未設定路由器上靜態 IP 或 DHCP 晚餐目的伺服器，並 DHCP 範圍不來源伺服器相同，則可能路由器將發行新的 IP 位址，目的地伺服器。 發生這種情形，如果重設轉送規則轉寄給新的目的伺服器的 IP 位址路由器的連接埠。  
  
###  <a name="BKMK_RemoveTheSourceServer"></a>移除和重新安排來源伺服器  
 關閉來源伺服器，並將它拔除從網路。 我們建議您無法重新格式化來源伺服器至少一個星期了，以確保，所需的所有資料都移轉到目的伺服器。 確認您的所有資料都移轉之後，您可以重新安裝此伺服器網路上的次要伺服器的其他工作，如果需要的話。  
  
> [!NOTE]
>  您降級並移除來源伺服器之後，請重新開機目的伺服器。  
  
 您降級來源伺服器之後，它不健全狀態。 如果您想要重新安排來源伺服器，最簡單的方式是重新設定，安裝伺服器作業系統，然後再設定，以便使用其他伺服器以搭配。
