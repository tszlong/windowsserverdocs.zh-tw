---
title: 準備遠端桌面的虛擬機器
description: 取得您的 Vm 準備好進行遠端桌面元件
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/21/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fc39dff-61ca-4eba-81ab-52289081bead
author: lizap
manager: dongill
ms.openlocfilehash: cb06963c3ae51fb9337827c7b29b93b8c2736c16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871879"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>建立遠端桌面的虛擬機器

>適用於：Windows Server （半年通道），Windows Server 2016

您可以在虛擬機器或實體伺服器上安裝遠端桌面服務元件。 

第一個步驟是[在 Azure 中建立 Windows Server 虛擬機器](/azure/virtual-machines/windows/quick-create-portal)。 您會想要建立三個 Vm： 一個用於 RD 工作階段主機，一個用於連線代理人，一個用於 RD Web 和 RD 閘道。 若要確保您的 RDS 部署的可用性，請建立可用性設定 (下方**高可用性**VM 建立程序中) 和群組，因為可用性設定組的多個 Vm。
 
建立 Vm 之後，使用下列步驟以準備 rds。

1.  連接到虛擬機器使用遠端桌面連線 (RDC) 用戶端：  
    1.  在 Azure 入口網站中開啟 [資源群組] 檢視中，，然後按一下要用於部署的資源群組。  
    2.  選取新的 RDSH 虛擬機器 (例如，Contoso-Sh1)。  
    3.  按一下  **Connect > 開啟**若要開啟 遠端桌面用戶端。  
    4.  在用戶端中，按一下**Connect**，然後按一下**使用另一個使用者帳戶**。 輸入本機系統管理員帳戶的使用者名稱和密碼。  
    5.  按一下 **是**時看到有關憑證的警告。  
2.  啟用遠端管理：  
    1.  在 [伺服器管理員] 中，按一下**本機伺服器 > 遠端管理 （停用） 的目前設定**。  
    2.  選取 **啟用此伺服器的遠端管理**。  
    3.  按一下 [確定] 。  
3.  選擇性：您可以暫時設定不會自動下載並安裝更新的 Windows 更新。 這有助於防止變更 」 和 「 系統重新啟動，而您將 RDSH 伺服器部署。  
    1.  在 [伺服器管理員] 中，按一下**本機伺服器 > 目前的 Windows Update 設定**。  
    2.  選取 [**進階選項] > 延遲升級**。   
4.  您可以將伺服器加入網域：  
    1.  在 [伺服器管理員] 中，按一下**本機伺服器 > 工作群組的目前設定**。  
    2.  按一下 **變更 > 網域**，然後輸入網域名稱 (例如，Contoso.com)。  
    3.  輸入網域系統管理員認證。  
    4.  重新啟動虛擬機器。  
5.  RD Web 和 GW 虛擬機器重複步驟 1 到 4。  
6.  針對 RD 連線代理人虛擬機器重複步驟 1 到 4。  
7.  初始化並格式化在 RD 連線代理人虛擬機器上連接的磁碟：  
    1.  連線到 RD 連線代理人虛擬機器 （上述步驟 1）。  
    2.  在 [伺服器管理員] 中，按一下**工具 > 電腦管理**。  
    3.  按一下 **磁碟管理**。  
    4.  選取連接的磁碟，然後**MBR （主開機記錄）**，然後按一下**確定**。  
    5.  以滑鼠右鍵按一下新的磁碟 (標示**未配置**)，按一下 **新增簡單磁碟區**。  
    6.  在 **新增簡單磁碟區**精靈，接受預設值，但提供的適用名稱**磁碟區標籤**（例如共用）。  
8.  RD 連線代理人虛擬機器上建立檔案共用的使用者設定檔磁碟和憑證：   
    1.  開啟檔案總管 中，按一下**這部電腦**，並開啟您加入的檔案共用的磁碟。  
    2.  按一下 **首頁**並**新資料夾**。  
    3.  輸入使用者磁碟資料夾名稱，例如**UserDisks**。  
    4.  以滑鼠右鍵按一下新的資料夾，然後按一下 **屬性 > 共用 > 進階共用**。  
    5.  選取 **共用此資料夾**然後按一下**權限**。  
    6.  選取  **Everyone**，然後按一下**移除**。 現在，請按一下**新增**，輸入**Domain Admins**，然後按一下**確定**。  
    7.  選取 **允許完整控制**，然後按一下**確定 > 確定 > 關閉**。  
    8.  重複步驟 c。 為 g。 若要建立憑證的共用的資料夾。   


