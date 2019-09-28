---
title: 新增 RD 工作階段主機伺服器陣列來向外延展 RDS 部署
description: 將另一部 RD 工作階段主機新增至 RDS 環境。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: da0dbd4332cd05d580c2b1f4dc5eb0734b36b13e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403889"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>新增 RD 工作階段主機伺服器陣列來向外延展遠端桌面服務部署

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

您可以透過新增遠端桌面工作階段主機 (RDSH) 伺服器陣列來改善 RDS 部署的可用性和延展性。   
  
 
使用下列步驟，將另一部 RD 工作階段主機新增至您的部署：  
  
1. 建立一部伺服器來裝載第二部 RD 工作階段主機。 如果您使用的是 Azure 虛擬機器，請務必將新的 VM 包含在保留第一部 RD 工作階段主機的相同可用性設定組中。
2. 在新的伺服器或虛擬機器上啟用遠端管理：
   1. 在 [伺服器管理員] 中，按一下 [本機伺服器] > 遠端管理目前的設定 (停用)  。 
   2. 選取 [為此伺服器啟用遠端管理]  ，然後按一下 [確定]  。 
   3. 選擇性：您可以暫時設定 Windows Update 不要自動下載並安裝更新。 這有助於在部署 RDSH 伺服器時防止變更和系統重新啟動。 在 [伺服器管理員] 中，按一下 [本機伺服器] > Windows Update 目前的設定  。 按一下 [進階選項] > [延遲升級]  。 
3. 將伺服器或 VM 新增至網域：
   1. 在 [伺服器管理員] 中，按一下 [本機伺服器] > 工作群組目前的設定  。 
   2. 按一下 [變更] > [網域]  ，然後輸入網域名稱 (例如 Contoso.com)。 
   3. 輸入網域系統管理員認證。 
   4. 重新啟動伺服器或 VM。
4. 將 RD 工作階段主機新增到伺服器陣列：
   >[!NOTE] 
   > 步驟 1：只有在您要將 VM 用於 RDMS，而且 RDMS 虛擬機器還沒有獲指派 IP 位址時，才需要建立公用 IP 位址。
   
   1. 為執行遠端桌面管理服務 (RDMS) 的虛擬機器建立公用 IP 位址。 RDMS 虛擬機器通常是執行第一個 RD 連線代理人角色執行個體的虛擬機器。  
       1. 在 Azure 入口網站中，按一下 [瀏覽] > [資源群組]  、按一下用於部署的資源群組，然後按一下 RDMS 虛擬機器 (例如 Contoso-Cb1)。  
       2. 按一下 [設定] > [網路介面]  ，然後按一下對應的網路介面。   
       3. 按一下 [設定] > [IP 位址]  。
       4. 針對 [公用 IP 位址]  選取 [已啟用]  ，然後按一下 [IP 位址]  。   
       5. 如果您目前有想要使用的公用 IP 位址，請從清單中加以選取。 否則，請按一下 [新建]  、輸入名稱，然後依序按一下 [確定]  和 [儲存]  。   
   2. 登入 RDMS。
   3. 將 RDSH 伺服器新增到伺服器管理員：   
       1. 啟動 [伺服器管理員]，然後按一下 [管理] > [新增伺服器]  。   
       2. 在 [新增伺服器] 對話方塊中，按一下 [立即尋找]  。   
       3. 選取您想要用於 RD 工作階段主機的伺服器，或新建立的虛擬機器 (例如，Contoso-Sh2)，然後按一下 [確定]  。
   4. 將 RDSH 伺服器新增到部署
       1. 啟動 [伺服器管理員]。  
       2. 按一下 [遠端桌面服務] > [概觀] > [部署伺服器] > [工作] > [新增 RD 工作階段主機伺服器]  。   
       3. 選取新的伺服器 (例如 Contoso-Sh2)，然後按 [下一步]  。  
       4. 在 [確認] 頁面上選取 [視需要重新啟動遠端電腦]  ，然後按一下 [新增]  。   
   5. 將 RDSH 伺服器新增到集合伺服器陣列：
       1. 啟動 [伺服器管理員]。   
       2. 按一下 [遠端桌面服務]  ，然後按一下您想要在其中新增新建立之 RDSH 伺服器 (例如 ContosoDesktop) 的集合。   
       3. 在 [主機伺服器]  底下，按一下 [工作] > [新增 RD 工作階段主機伺服器]  。   
       4. 選取新建立的伺服器 (例如 Contoso-Sh2)，然後按 [下一步]  。   
       5. 在 [確認] 頁面上，按一下 [新增]  。   

