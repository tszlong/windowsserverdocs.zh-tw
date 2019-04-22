---
title: 步驟 2：Windows Server Essentials 安裝為新的複本網域控制站
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816459"
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>步驟 2：Windows Server Essentials 安裝為新的複本網域控制站

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本節說明如何使用來安裝 Windows Server Essentials 和 Windows Server 2012 R2 Standard （已啟用 「 Windows Server Essentials 體驗角色） 為網域控制站。  
  
 具有多達 25 名使用者及 50 台裝置的環境，您可以依照本指南，以從舊版的 Windows SBS 移轉到 Windows Server Essentials 中的步驟。 具有最多 100 位使用者和 200 台裝置的環境，您可以依照相同的指引移轉到 Windows Server 2012 R2 Standard 和 Datacenter 版本已安裝的 Windows Server Essentials 體驗角色。 本文件將涵蓋這兩種案例。  
  
> [!IMPORTANT]
>  如果您移轉到 Windows Server Essentials 時，下列的錯誤訊息加入至事件記錄檔之前從網路移除來源伺服器在 21 天寬限期內的每一天。 21 天寬限期之後，會關閉來源伺服器。 <br> **FSMO 角色檢查偵測環境不符合授權原則中的情況。管理伺服器必須具有網域主控站和網域命名主要 Active Directory 角色。請立即將 Active Directory 角色移至管理伺服器。此伺服器將會自動關閉若此問題修正的時間，第一次偵測到此狀況的 21 天內**。   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>在目的地伺服器上安裝 Windows Server Essentials 或 Windows Server 2012 R2 Standard  
  
1.  安裝 Windows Server Essentials 或 Windows Server 2012 R2 Standard 中的指示已啟用 Windows Server Essentials 體驗角色[安裝和設定 Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  
  
    > [!NOTE]
    >  如果「設定 Windows Server Essentials 精靈」啟動，請將它取消。  
  
2.  從來源伺服器移轉 FSMO 角色。  
  
    > [!NOTE]
    >  如果 Windows Server Essentials 網域中唯一的網域控制站，則 FSMO 角色會自動移到時降級來源伺服器執行 Windows Server Essentials 的伺服器。  
  
3.  開啟伺服器管理員，然後執行「新增角色及功能」精靈。  
  
4.  如果未安裝，請新增 Windows Server Essentials 經驗角色。  
  
5.  安裝 Windows Server Essentials 體驗角色之後，[設定 Windows Server Essentials] 工作會出現在通知區域中。 按一下工作以啟動「設定 Windows Server Essentials」精靈。  
  
6.  依照指示完成 Windows Server Essentials 的設定。 執行精靈之前，請執行下列作業：  
  
    -   如有需要，請變更伺服器名稱，因為您完成「設定 Windows Server Essentials 精靈」之後，就無法再變更名稱。  
  
    -   請確定伺服器的時間和設定正確無誤。  
  
7.  請依照下列步驟確認安裝成功：  
  
    1.  開啟 [儀表板]。  
  
    2.  按一下 [使用者]  索引標籤，然後確認已列出您 Active Directory 中的使用者帳戶。  
  
### <a name="transfer-the-operations-master-roles"></a>轉移操作主機角色  
 操作主機 （也稱為彈性單一主機操作或 FSMO） 角色必須被傳輸從來源伺服器到目的地伺服器的目的地伺服器上安裝 Windows Server Essentials 的 21 天內。  
  
##### <a name="to-transfer-the-operations-master-roles"></a>轉移操作主機角色  
  
1.  在目的地伺服器上，以系統管理員身分開啟 [命令提示字元] 視窗。 請參閱 [以系統管理員身分開啟命令提示字元視窗](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)。  
  
2.  在命令提示字元下，輸入 **NETDOM QUERY FSMO**，然後按 ENTER。  
  
3.  在命令提示字元下，輸入 **ntdsutil**，然後按 ENTER。  
  
4.  在 **ntdsutil** 命令提示字元中，輸入下列命令：  
  
    1.  輸入 **activate instance NTDS**，然後按 ENTER 鍵。  
  
    2.  輸入 **角色**，然後按 ENTER。  
  
    3.  輸入 **連線**，然後按 ENTER。  
  
    4.  型別**連接到伺服器** *< 伺服器名稱\>* (其中 *< ServerName\>* 是目的地伺服器的名稱)，然後按 ENTER 鍵。  
  
    5.  在命令提示字元中輸入 **q**，然後按 ENTER。  
  
        1.  輸入 **transfer PDC**，按 ENTER 鍵，然後在 [角色轉移確認] 對話方塊中按一下 [是]。  
  
        2.  輸入 **傳輸基礎結構主機**，按 ENTER，然後在 [角色轉移確認] 對話方塊中按一下 [是]。  
  
        3.  輸入 **傳輸命名主機**，按 ENTER，然後在 [角色轉移確認]  對話方塊中按一下 [是]  。  
  
        4.  輸入 **傳輸 RID 主機**，按 ENTER，然後在 [角色轉移確認] 對話方塊中按一下 [是]。  
  
        5.  輸入 **傳輸架構主機**，按 ENTER，然後在 [角色轉移確認]  對話方塊中按一下 [是]  。  
  
    6.  輸入 **q**，然後按 ENTER 鍵，直到您返回命令提示字元。  
  
> [!NOTE]
>  從網路上的任何伺服器，您都可以確認操作主機角色已轉移到目的地伺服器。 以系統管理員身分開啟 [命令提示字元] 視窗 (如需詳細資訊，請參閱 [以系統管理員身分開啟命令提示字元視窗](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx))。 輸入 **netdom query fsmo**，然後按 ENTER。  
  
## <a name="next-steps"></a>後續步驟  
 您已安裝 Windows Server Essentials 為新的複本網域控制站。 現在請移至[步驟 3:將電腦加入新的 Windows Server Essentials 伺服器](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md)。  
  
若要檢視所有步驟，請參閱[移轉至 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

