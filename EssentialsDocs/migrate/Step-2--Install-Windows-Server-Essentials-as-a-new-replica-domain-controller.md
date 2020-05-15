---
title: 步驟 2：安裝 Windows Server Essentials 為新的複本網域控制站
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: c7ccfc34-63fd-436b-a1cd-e05810f60bfe
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d5d86f5ae25c67ffb9e0f00fc5d373e6a095df98
ms.sourcegitcommit: 2f072c0c02e3e0deae331ca64b375d63b89d0522
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2020
ms.locfileid: "83404551"
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>步驟 2：安裝 Windows Server Essentials 為新的複本網域控制站

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials

本節說明如何安裝 Windows Server Essentials 和 Windows Server 2012 R2 Standard （已啟用 Windows Server Essentials 體驗角色）作為網域控制站。  
  
 對於多達25位使用者和50裝置的環境，您可以依照本指南中的步驟，從舊版的 Windows SBS 遷移到 Windows Server Essentials。 針對高達100使用者和200裝置的環境，您可以遵循相同的指導方針，遷移至已安裝 Windows Server Essentials 體驗角色之 Windows Server 2012 R2 的 Standard 和 Datacenter 版本。 本文件將涵蓋這兩種案例。  
  
> [!IMPORTANT]
>  如果您遷移到 Windows Server Essentials，在21天的寬限期內，每天會在事件記錄檔中新增下列錯誤訊息，直到您從網路中移除來源伺服器為止。 21 天寬限期之後，會關閉來源伺服器。 <br> **FSMO 角色檢查偵測到您的環境中有不符合授權原則的條件。管理伺服器必須保留網域主控站和網網域命名主機 Active Directory 角色。請立即將 Active Directory 角色移至管理伺服器。如果未在第一次偵測到此狀況的21天內修正此問題，將會自動關閉這部伺服器**。   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>在目的地伺服器上安裝 Windows Server Essentials 或 Windows Server 2012 R2 Standard  
  
1.  遵循[安裝和設定 Windows Server essentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)中的指示，安裝 Windows server Essentials 或 windows Server 2012 R2 Standard，並啟用 Windows Server essentials 體驗角色。  
  
    > [!NOTE]
    >  如果「設定 Windows Server Essentials 精靈」啟動，請將它取消。  
  
2.  從來源伺服器移轉 FSMO 角色。  
  
    > [!NOTE]
    >  如果 Windows Server Essentials 是網域中唯一的網域控制站，則當您降級來源伺服器時，FSMO 角色會自動移至執行 Windows Server Essentials 的伺服器。  
  
3.  開啟伺服器管理員，然後執行「新增角色及功能」精靈。  
  
4.  如果未安裝，請新增 Windows Server Essentials 經驗角色。  
  
5.  安裝 Windows Server Essentials 體驗角色之後，[設定 Windows Server Essentials] 工作會出現在通知區域中。 按一下工作以啟動「設定 Windows Server Essentials」精靈。  
  
6.  依照指示完成 Windows Server Essentials 的設定。 執行精靈之前，請執行下列作業：  
  
    -   如有需要，請變更伺服器名稱，因為您完成「設定 Windows Server Essentials 精靈」之後，就無法再變更名稱。  
  
    -   請確認伺服器的時間和設定是否正確。  
  
7.  請依照下列步驟確認安裝成功：  
  
    1.  開啟 [儀表板]。  
  
    2.  按一下 [使用者]**** 索引標籤，然後確認已列出您 Active Directory 中的使用者帳戶。  
  
### <a name="transfer-the-operations-master-roles"></a>轉移操作主機角色  
 操作主機（也稱為彈性單一主機操作或 FSMO）角色必須在目的地伺服器上安裝 Windows Server Essentials 的21天內，從來源伺服器轉移到目的地伺服器。  
  
##### <a name="to-transfer-the-operations-master-roles"></a>轉移操作主機角色  
  
1.  在目的地伺服器上，以系統管理員身分開啟 [命令提示字元] 視窗。 請參閱 [以系統管理員身分開啟命令提示字元視窗](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)。  
  
2.  在命令提示字元下，輸入 **NETDOM QUERY FSMO**，然後按 ENTER。  
  
3.  在命令提示字元下，輸入 **ntdsutil**，然後按 ENTER。  
  
4.  在 **ntdsutil** 命令提示字元中，輸入下列命令：  
  
    1.  輸入 **activate instance NTDS**，然後按 ENTER 鍵。  
  
    2.  輸入 **角色**，然後按 ENTER。  
  
    3.  輸入 **連線**，然後按 ENTER。  
  
    4.  輸入**connect to server** *<servername \> * （其中 *<ServerName \> *是目的地伺服器的名稱），然後按 enter。  
  
    5.  在命令提示字元中輸入 **q**，然後按 ENTER。  
  
        1.  輸入 **transfer PDC**，按 ENTER 鍵，然後在 [角色轉移確認]**** 對話方塊中按一下 [是]****。  
  
        2.  輸入 **傳輸基礎結構主機**，按 ENTER，然後在 [角色轉移確認]**** 對話方塊中按一下 [是]****。  
  
        3.  輸入 **傳輸命名主機**，按 ENTER，然後在 [角色轉移確認]**** 對話方塊中按一下 [是]****。  
  
        4.  輸入 **傳輸 RID 主機**，按 ENTER，然後在 [角色轉移確認]**** 對話方塊中按一下 [是]****。  
  
        5.  輸入 **傳輸架構主機**，按 ENTER，然後在 [角色轉移確認]**** 對話方塊中按一下 [是]****。  
  
    6.  輸入 **q**，然後按 ENTER 鍵，直到您返回命令提示字元。  
  
> [!NOTE]
>  從網路上的任何伺服器，您都可以確認操作主機角色已轉移到目的地伺服器。 以系統管理員身分開啟 [命令提示字元] 視窗 (如需詳細資訊，請參閱 [以系統管理員身分開啟命令提示字元視窗](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx))。 輸入 **netdom query fsmo**，然後按 ENTER。  
  
## <a name="next-steps"></a>後續步驟  
 您已將 Windows Server Essentials 安裝為新的複本網域控制站。 現在請移至[步驟3：將電腦加入新的 Windows Server Essentials 伺服器](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  
  
若要查看所有步驟，請參閱[遷移至 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

