---
title: "步驟 2： 與全新的複本網域控制站安裝 Windows Server Essentials"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7ccfc34-63fd-436b-a1cd-e05810f60bfe
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 757012b7d1a57a001e3b55cdc0604b63852a3d3c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>步驟 2： 與全新的複本網域控制站安裝 Windows Server Essentials

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

本節 （與 Windows Server Essentials 體驗角色支援） 安裝 Windows Server Essentials 和 Windows Server 2012 標準 R2 網域控制站的方式。  
  
 針對環境中最多 25 位使用者與 50 個裝置，您可以依照本指南從舊版的 Windows SBS 移轉到 Windows Server Essentials。 最多 100 使用者和 200 裝置的環境，您可以依照相同的指導方針與 Windows Server Essentials 體驗角色安裝移轉到 Standard 和 Datacenter 版本的 Windows Server 2012 R2。 本文件中所涵蓋這兩個案例。  
  
> [!IMPORTANT]
>  如果您移轉到 Windows Server Essentials，下列錯誤訊息被新增至事件登入期間 21 日優惠直到您移除您的網路來源伺服器每一天。 21 日優惠期間之後來源伺服器會關閉。 <br> **FSMO 角色檢查偵測條件授權原則遵守退出的環境中。管理伺服器必須按住主要網域控制站和網域命名主要 Active Directory 角色。請移至管理伺服器的 Active Directory 角色現在。此伺服器將會自動關閉如果無法修正問題 21 天從第一次此條件偵測到的時間在**。   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>目的地伺服器上安裝 Windows Server Essentials 或 Windows Server 2012 R2 標準  
  
1.  安裝 Windows Server Essentials 或 Windows Server 2012 標準 R2 依照下列指示支援 Windows Server Essentials 體驗角色[安裝與設定 Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  
  
    > [!NOTE]
    >  如果 [設定 Windows Server Essentials 精靈時限，取消。  
  
2.  從您的來源伺服器傳輸故障。  
  
    > [!NOTE]
    >  Windows Server Essentials 是否只網域控制站網域中的，FSMO 角色會自動移到執行 Windows Server Essentials 當您降級來源伺服器伺服器。  
  
3.  打開伺服器管理員並執行新增角色與功能精靈。  
  
4.  如果無法安裝，新增 Windows Server Essentials 體驗角色。  
  
5.  安裝 Windows Server Essentials 體驗角色之後，設定 Windows Server Essentials 工作會顯示在通知區域中。 按一下稍設定 Windows Server Essentials 精靈中的工作。  
  
6.  請依照下列指示完成 Windows Server Essentials 的設定。 您在執行精靈之前，請執行下列動作：  
  
    -   變更伺服器名稱如有需要因為您已經完成設定 Windows Server Essentials 精靈之後，您無法變更名稱。  
  
    -   確保伺服器的時間，並設定正確。  
  
7.  請確認安裝，如下所示：  
  
    1.  打開儀表板。  
  
    2.  按一下**使用者**索引標籤，然後確認 [帳號，您的 Active Directory 中已列出。  
  
### <a name="transfer-the-operations-master-roles"></a>傳送操作主機角色  
 操作 （也稱為彈性的單一主機操作或 FSMO） 主機必須將它們傳輸來源伺服器的目的地伺服器 21 天目的伺服器上安裝 Windows Server Essentials 的中。  
  
##### <a name="to-transfer-the-operations-master-roles"></a>若要傳送操作主機角色  
  
1.  目的伺服器，開放 [以系統管理員身分在命令提示字元視窗。 查看[開放命令提示字元視窗中，系統管理員的身分，以](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)。  
  
2.  在命令提示字元中，輸入**NETDOM 查詢 FSMO**，然後按 ENTER 鍵。  
  
3.  在命令提示字元中，輸入**ntdsutil**，然後按 ENTER 鍵。  
  
4.  在**ntdsutil**命令提示字元中，輸入下列命令：  
  
    1.  輸入**啟動執行個體 NTDS**，然後按 ENTER 鍵。  
  
    2.  輸入**角色**，然後按 ENTER 鍵。  
  
    3.  輸入**連接**，然後按 ENTER 鍵。  
  
    4.  輸入**連接伺服器** *< ServerName\ >* (其中*< ServerName\ >*目的伺服器的名稱)，然後按 ENTER 鍵。  
  
    5.  在命令提示字元中，輸入**q**，然後按 ENTER 鍵。  
  
        1.  輸入**傳輸 PDC**，按下 ENTER，然後按一下 [**是**中**角色傳輸確認**] 對話方塊。  
  
        2.  輸入**傳輸基礎結構主機**，按下 ENTER，然後按一下 [**是**中**角色傳輸確認**] 對話方塊。  
  
        3.  輸入**傳輸命名主機**，按下 ENTER，然後按一下 [**是**中**角色傳輸確認**] 對話方塊。  
  
        4.  輸入**傳輸 RID 主機**，按下 ENTER，然後按一下 [**是**中**角色傳輸確認**] 對話方塊。  
  
        5.  輸入**傳輸架構主機**，按下 ENTER，然後按一下 [**是**中**角色傳輸確認**] 對話方塊。  
  
    6.  輸入**q**，然後按 ENTER 鍵，直到您回到命令提示字元。  
  
> [!NOTE]
>  從網路上的任何伺服器，您就可以驗證操作主機角色已將轉移到目的伺服器。 打開命令提示字元視窗以系統管理員 (如需詳細資訊，請查看[若要打開命令提示字元視窗以系統管理員身分](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx))。 輸入**netdom 查詢 fsmo**，然後按 ENTER 鍵。  
  
## <a name="next-steps"></a>後續步驟  
 您已安裝 Windows Server Essentials 的全新複本網域控制站為。 立即移至[執行 「 步驟 3： 電腦加入新的 Windows Server Essentials 伺服器](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  
  
若要檢視所有的步驟，請查看[Windows Server essentials 移轉](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

