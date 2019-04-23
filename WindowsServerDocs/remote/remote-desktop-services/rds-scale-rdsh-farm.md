---
title: 新增 RD 工作階段主機伺服器陣列來相應放大您的 RDS 部署
description: 新增至 RDS 環境的第二個 RD 工作階段主機。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: d243994a68c0bf4f0584f68475a185acb9cb73d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865489"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>新增 RD 工作階段主機伺服器陣列來相應放大您的遠端桌面服務部署

>適用於：Windows Server （半年通道），Windows Server 2016

您可以改善可用性和延展性的 RDS 部署加上的遠端桌面工作階段主機 (RDSH) 伺服器陣列。   
  
 
若要新增至您的部署的另一部 RD 工作階段主機使用下列步驟：  
  
1. 建立裝載第二個 RD 工作階段主機伺服器。 如果您使用 Azure 虛擬機器，請務必包含新的 VM 相同的可用性設定組，保留您的第一個 RD 工作階段主機。
2. 啟用新的伺服器或虛擬機器上的遠端管理：
   1. 在 [伺服器管理員] 中，按一下**本機伺服器 > 遠端管理 （停用） 的目前設定**。 
   2. 選取 **啟用此伺服器的遠端管理**，然後按一下**確定**。 
   3. 選擇性：您可以暫時設定不會自動下載並安裝更新的 Windows 更新。 這有助於防止變更 」 和 「 系統重新啟動，而您將 RDSH 伺服器部署。 在 [伺服器管理員] 中，按一下**本機伺服器 > 目前的 Windows Update 設定**。 按一下 [**進階選項] > 延遲升級**。 
3. 加入網域的伺服器或 vm:
   1. 在 [伺服器管理員] 中，按一下**本機伺服器 > 工作群組的目前設定**。 
   2. 按一下 **變更 > 網域**，然後輸入網域名稱 (例如，Contoso.com)。 
   3. 輸入網域系統管理員認證。 
   4. 重新啟動伺服器或 vm。
4. 加入新的 RD 工作階段主機伺服器陣列：
>[!NOTE] 
> 步驟 1： 建立 RDMS 虛擬機器的公用 IP 位址，才需要，如果您要用於 RDMS 中的 vm，而且它還沒有指派的 IP 位址。
   
   1. 建立執行遠端桌面管理服務 (RDMS) 的虛擬機器的公用 IP 位址。 RDMS 虛擬機器通常會執行 「 RD 連線代理人 」 角色的第一個執行個體的虛擬機器。  
       1. 在 Azure 入口網站中，按一下**瀏覽 > 資源群組**，按一下 部署的資源群組，然後按一下 RDMS 虛擬機器 (例如，Contoso-Cb1)。  
       2. 按一下 **設定 > 網路介面**，然後按一下 對應的網路介面。   
       3. 按一下 **設定 > IP 位址**。
       4. 針對**公用 IP 位址**，選取**已啟用**，然後按一下**IP 位址**。   
       5. 如果您有想要使用的現有公用 IP 位址，請從清單中選取。 否則，請按一下**新建**，輸入名稱，然後按一下  **確定** ，然後**儲存**。   
   2. 登入 RDMS。
   3. 將 RDSH 伺服器新增到伺服器管理員：   
       1. 啟動 [伺服器管理員] 中，按一下**管理 > 新增伺服器**。   
       2. 在 [新增伺服器] 對話方塊中，按一下**立即尋找**。   
       3. 選取您想要使用的 RD 工作階段主機或新建立的虛擬機器 (例如，Contoso-Sh2)，然後按一下的伺服器**確定**。
   4. 將 RDSH 伺服器加入至部署
       1. 啟動 [伺服器管理員]。  
       2. 按一下 **遠端桌面服務 > 概觀 > 部署伺服器 > 工作 > 加入 RD 工作階段主機伺服器**。   
       3. 選取新的伺服器 (例如，Contoso-Sh2)，然後再按一下**下一步**。  
       4. 在 [確認] 頁面上選取**視需要重新啟動遠端電腦**，然後按一下**新增**。   
   5. 將 RDSH 伺服器加入至集合伺服器陣列中：
       1. 啟動 [伺服器管理員]。   
       2. 按一下  **Remote Desktop Services**然後按一下您想要新增新建立的 RDSH 伺服器 (例如 ContosoDesktop) 的集合。   
       3. 底下**主機伺服器**，按一下**工作 > 加入 RD 工作階段主機伺服器**。   
       4. 選取新建立的伺服器 (例如，Contoso-Sh2)，然後再按一下**下一步**。   
       5. 在 確認 頁面中，按一下 **新增**。   

