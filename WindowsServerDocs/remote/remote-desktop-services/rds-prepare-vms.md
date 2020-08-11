---
title: 準備遠端桌面的虛擬機器
description: 為遠端桌面元件準備您的 VM
ms.author: elizapo
ms.date: 07/21/2017
ms.topic: article
ms.assetid: 2fc39dff-61ca-4eba-81ab-52289081bead
author: lizap
manager: dongill
ms.openlocfilehash: 19aa4547df44b5b99de9385e0b9cbe697d4c51f9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971535"
---
# <a name="prepare-your-virtual-machines-for-remote-desktop"></a>準備遠端桌面的虛擬機器

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

您可以在實體伺服器或虛擬機器上安裝遠端桌面服務元件。

第一個步驟是[在 Azure 中建立 Windows Server 虛擬機器](/azure/virtual-machines/windows/quick-create-portal)。 您需要建立三部 VM：一部用於 RD 工作階段主機，一部用於連線代理人，一部用於 RD Web 和 RD 閘道。 為了確保 RDS 部署的可用性，請建立可用性設定組 (在 VM 建立程序的 [高可用性]  下)，並將多個 VM 分組到該可用性設定組中。

建立 VM 之後，請使用下列步驟來為 RDS 準備這些 VM。

1.  使用遠端桌面連線 (RDC) 用戶端連線到虛擬機器：
    1.  在 Azure 入口網站中，開啟 [資源群組] 檢視，然後按一下要用於部署的資源群組。
    2.  選取新的 RDSH 虛擬機器 (例如 Contoso-Sh1)。
    3.  按一下 [連線] > [開啟]  以開啟遠端桌面用戶端。
    4.  在用戶端，按一下 [連線]  ，然後按一下 [Use another user account] \(使用其他使用者帳戶\)  。 輸入本機系統管理員帳戶的使用者名稱和密碼。
    5.  出現憑證相關警告時，請按一下 [是]  。
2.  啟用遠端管理：
    1.  在 [伺服器管理員] 中，按一下 [本機伺服器] > 遠端管理目前的設定 (停用)  。
    2.  選取 [Enable remote management for this server] \(為此伺服器啟用遠端管理\)  。
    3.  按一下 [確定]  。
3.  選擇性：您可以暫時設定 Windows Update 不要自動下載並安裝更新。 這有助於在部署 RDSH 伺服器時防止變更和系統重新啟動。
    1.  在 [伺服器管理員] 中，按一下 [本機伺服器] > Windows Update 目前的設定  。
    2.  選取 [進階選項] > [延遲升級]  。
4.  將伺服器新增至網域：
    1.  在 [伺服器管理員] 中，按一下 [本機伺服器] > 工作群組目前的設定  。
    2.  按一下 [變更] > [網域]  ，然後輸入網域名稱 (例如 Contoso.com)。
    3.  輸入網域系統管理員認證。
    4.  重新啟動虛擬機器。
5.  針對 RD Web 和 GW 虛擬機器重複步驟 1 到 4。
6.  針對 RD 連線代理人虛擬機器重複步驟 1 到 4。
7.  將 RD 連線代理人虛擬機器上已連結的磁碟初始化並格式化：
    1.  連線到 RD 連線代理人虛擬機器 (上述步驟 1)。
    2.  在 [伺服器管理員] 中，按一下 [工具] > [電腦管理]  。
    3.  按一下 [磁碟管理]  。
    4.  依序選取已連結的磁碟和 [MBR (主開機記錄)]  ，然後按一下 [確定]  。
    5.  以滑鼠右鍵按一下新磁碟 (標示為 [未配置]  )，然後按一下 [新增簡單磁碟區]  。
    6.  在 [新增簡單磁碟區精靈]  中接受預設值，但針對 [磁碟區標籤]  提供適用名稱 (例如 Shares)。
8.  在 RD 連線代理人虛擬機器上，為使用者設定檔磁碟和憑證建立檔案共用：
    1.  開啟檔案總管，按一下 [此電腦]  ，然後開啟您為檔案共用新增的磁碟。
    2.  依序按一下 [首頁]  和 [新增資料夾]  。
    3.  輸入使用者磁碟資料夾的名稱，例如 **UserDisks**。
    4.  以滑鼠右鍵按一下新資料夾，然後按一下 [內容] > [共用] > [進階共用]  。
    5.  選取 [共用此資料夾]  ，然後按一下 [權限]  。
    6.  選取 [Everyone]  ，然後按一下 [移除]  。 現在按一下 [新增]  ，輸入 **Domain Admins**，然後按一下 [確定]  。
    7.  選取 [允許完全控制]  ，然後按一下 [確定] > [確定] > [關閉]  。
    8.  重複步驟 c. 到 g. 以建立憑證的共用資料夾。


