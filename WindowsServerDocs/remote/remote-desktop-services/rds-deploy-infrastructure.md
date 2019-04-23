---
title: 部署遠端桌面環境
ms.custom: na
ms.prod: windows-server-threshold
description: 若要部署遠端桌面環境的基本步驟。
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: acfdd99fa67e218f58fe650de5607f2a5ba97bf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833829"
---
# <a name="deploy-your-remote-desktop-environment"></a>部署遠端桌面環境

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用下列步驟來部署環境中的遠端桌面伺服器。 您可以在實體機器或虛擬機器，取決於所建立的內部部署、 雲端式或混合式環境上安裝伺服器角色。 

如果您使用任何遠端桌面服務伺服器的虛擬機器，請確定您已[備妥這些虛擬機器](rds-prepare-vms.md)。
  
  
1.  新增您要用於遠端桌面服務伺服器管理員的所有伺服器：  
    1.  在 伺服器管理員中，按一下**管理 > 新增伺服器**。  
    2.  按一下 **立即尋找**。  
    3.  按一下 部署 （例如，Contoso Cb1、 Contoso-WebGw1 和 Contoso Sh1） 中的每一部伺服器，然後按一下**確定**。  
2.  建立工作階段為基礎的部署，以部署遠端桌面服務元件：  
    1.  在 [伺服器管理員] 中，按一下**管理 > 新增角色及功能**。  
    2.  按一下 **遠端桌面服務安裝**，**標準部署**，並**工作階段型桌面部署**。  
    3.  選取適當的伺服器，RD 連線代理人伺服器、 RD Web 存取伺服器和 RD 工作階段主機伺服器 (例如 Contoso Cb1、 Contoso-WebGw1 和 Contoso-SH1 分別)。  
    4.  選取 **需要時自動重新啟動目的地伺服器**，然後按一下**部署**。  
    5.  等待順利完成部署  
3.  新增 RD 授權伺服器：  
    1.  在 [伺服器管理員] 中，按一下**遠端桌面服務 > 概觀 > + RD 授權**。  
    2.  選取 RD 授權伺服器 （例如，Contoso Cb1） 的安裝所在的虛擬機器。  
    3.  按一下 **下一步**，然後按一下**新增**。  
4.  啟用 RD 授權伺服器，並將它新增至 License Servers 群組：  
    1.  在 [伺服器管理員] 中，按一下**工具 > 終端機服務 > 遠端桌面授權管理員**。  
    2.  在 [RD 授權管理員] 中，選取伺服器，然後**動作 > 啟用伺服器**。  
    3.  接受預設值，在啟用伺服器精靈接受預設值，直到您到達**公司資訊**頁面。 然後，輸入您的公司資訊。  
    4.  接受其餘頁面的預設值，直到最後一頁。 清除**立即啟動安裝授權精靈**，然後按一下**完成**。  
    5.  按一下 **動作 > 檢閱設定 > 新增到群組 > 確定**。 在 AAD DC 系統管理員群組中，使用者輸入認證，並註冊為 SCP。 如果您使用 Azure AD Domain Services，但您可以忽略任何警告或錯誤，此步驟可能無法運作。  
5.  新增 RD 閘道伺服器和憑證名稱：  
    1.  在 [伺服器管理員] 中，按一下**遠端桌面服務 > 概觀 > + RD 閘道**。  
    2.  在 [新增 RD 閘道伺服器精靈] 中，選取您要安裝 RD 閘道伺服器 (例如，Contoso-WebGw1) 的虛擬機器。  
    3.  輸入使用的 RD 閘道伺服器外部完整 DNS 名稱 (FQDN) 的 RD 閘道伺服器的 SSL 憑證名稱。 在 Azure 中，這是**DNS 名稱**加上標籤，並使用格式 servicename.location.cloudapp.azure.com。 比方說，contoso.westus.cloudapp.azure.com。  
    4.  按一下 **下一步**，然後按一下**新增**。
6.  建立並安裝在 RD 閘道和 RD 連線代理人伺服器的自我簽署的憑證。

       > [!NOTE]
       > 如果您是提供，而且安裝從受信任的憑證授權單位的憑證，請從步驟 h，以逐步執行每個角色的 k 執行程序。 您必須對每一個這些憑證的.pfx 檔案。
       
    1.  在 [伺服器管理員] 中，按一下**遠端桌面服務 > 概觀 > 工作 > 編輯部署內容**。  
    2.  依序展開**憑證**，然後捲動到資料表。 按一下  **RD 閘道 > 建立新憑證**。  
    3.  輸入憑證名稱，使用的 RD 閘道伺服器 (例如 contoso.westus.cloudapp.azure.com) 外部 FQDN，然後輸入密碼。  
    4.  選取 **儲存此憑證**然後瀏覽至您在上一個步驟中的憑證建立的共用資料夾。 （例如 \Contoso Cb1\Certificates。）  
    5.  輸入憑證 (比方說，ContosoRdGwCert) 的檔案名稱，然後按一下**儲存**。  
    6.  選取**允許憑證新增至目的地電腦上的受信任的根憑證授權單位憑證存放區**，然後按一下**確定**。  
    7.  按一下 **套用**，然後等待順利套用到 RD 閘道伺服器的憑證。  
    8.  按一下  **RD Web 存取 > 選取現有的憑證**。  
    9.  瀏覽至 RD 閘道伺服器 (例如 ContosoRdGwCert) 所建立的憑證，然後按一下**開啟**。  
    10. 輸入選取的憑證的密碼**允許憑證新增至目的地電腦的受信任的根憑證存放區**，然後按一下  **確定**。  
    11. 按一下 **套用**，然後等待順利套用到 RD Web 存取伺服器憑證。  
    12. 重複 substeps 1-11 for **RD 連線代理人-啟用單一登入**並**RD 連線代理人-發佈服務**，使用新的 RD 連線代理人伺服器的內部 FQDN憑證的名稱 (例如，Contoso-Cb1.Contoso.com)。  
7.  匯出自我簽署的公開憑證，並將它們複製到用戶端電腦。 如果您使用來自受信任的憑證授權單位憑證，您可以略過此步驟。  
    1.  啟動 [certlm.msc]。  
    2.  依序展開**個人**，然後按一下**憑證**。  
    3.  在右窗格中以滑鼠右鍵按一下 RD 連線代理人憑證供用戶端驗證，例如**Contoso Cb1.Contoso.com**。  
    4.  按一下 **所有的工作 > 匯出**。  
    5.  接受預設選項，在 [憑證匯出精靈] 中的接受預設值，直到您到達**要匯出的檔案**頁面。  
    6.  瀏覽至您建立的憑證，例如 \Contoso-Cb1\Certificates 的共用資料夾。  
    7.  輸入檔案名稱，例如 ContosoCbClientCert，然後按一下**儲存**。  
    8.  按 [下一步] ，然後按一下 [完成] 。  
    9.  重複子步驟 1-8 RD 閘道和 Web 憑證，(例如 contoso.westus.cloudapp.azure.com)，讓匯出的憑證執行適當的檔案名稱，例如**ContosoWebGwClientCert**。  
    10. 在檔案總管 中，瀏覽至儲存憑證的資料夾，例如 \Contoso-Cb1\Certificates。  
    11. 選取的兩個匯出的用戶端憑證，然後按一下滑鼠右鍵，然後按一下**複製**。  
    12. 在本機用戶端電腦上，貼上的憑證。  
8.  設定 RD 閘道和 RD 授權部署屬性：  
    1.  在 [伺服器管理員] 中，按一下**遠端桌面服務 > 概觀 > 工作 > 編輯部署內容**。  
    2.  依序展開**RD 閘道**並清除**本機位址略過 RD 閘道伺服器**選項。  
    3.  依序展開**RD 授權**，然後選取**每個使用者**  
    4.  按一下 [確定] 。  
10. 建立工作階段集合。 這些步驟會建立基本的集合。 請參閱[建立適用於桌上型電腦和應用程式執行的遠端桌面服務集合](rds-create-collection.md)如需集合的詳細資訊。
 
    1.  在 [伺服器管理員] 中，按一下**遠端桌面服務 > 集合 > 工作 > 建立工作階段集合**。  
    2.  輸入集合名稱 (例如 ContosoDesktop)。  
    3.  選取 RD 工作階段主機伺服器 (Contoso-Sh1)、 接受預設的使用者群組 （Contoso\Domain 使用者），並輸入通用命名慣例 (UNC) 路徑 (\Contoso-Cb1\UserDisks) 上面所建立的使用者設定檔磁碟。  
    4.  將大小上限，然後按一下**建立**。  
  

您現在已建立基本的遠端桌面服務基礎結構。 如果您需要建立高可用性部署，您可以加入[連線代理人叢集](rds-connection-broker-cluster.md)或是[第二個 RD 工作階段主機伺服器](rds-scale-rdsh-farm.md)。

